---
uid: web-forms/overview/older-versions-security/membership/creating-user-accounts-cs
title: "Criar contas de usuário (c#) | Microsoft Docs"
author: rick-anderson
description: "Neste tutorial, exploraremos usando a estrutura de associação (via SqlMembershipProvider) para criar novas contas de usuário. Veremos como criar novos nós..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/18/2008
ms.topic: article
ms.assetid: f175278c-6079-4d91-b9b4-2493ed43d9ec
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/membership/creating-user-accounts-cs
msc.type: authoredcontent
ms.openlocfilehash: d1bdec096b68a01c36f46765abef00aad319f2c2
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="creating-user-accounts-c"></a>Criar contas de usuário (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_05_CS.zip) ou [baixar PDF](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial05_CreatingUsers_cs.pdf)

> Neste tutorial, exploraremos usando a estrutura de associação (via SqlMembershipProvider) para criar novas contas de usuário. Veremos como criar novos usuários por meio de programação e por meio do ASP. Controle de CreateUserWizard interno do NET.


## <a name="introduction"></a>Introdução

No <a id="_msoanchor_1"> </a> [tutorial anterior](creating-the-membership-schema-in-sql-server-cs.md) instalamos o esquema do serviços de aplicativo em um banco de dados que adicionou as tabelas, exibições e procedimentos necessários para armazenados o `SqlMembershipProvider` e `SqlRoleProvider`. Isso criou a infraestrutura que será necessário para o restante dos tutoriais na série. Neste tutorial, exploraremos usando a estrutura de associação (por meio de `SqlMembershipProvider`) para criar novas contas de usuário. Veremos como criar novos usuários por meio de programação e por meio do ASP. Controle de CreateUserWizard interno do NET.

Além de aprender a criar novas contas de usuário, também precisamos atualizar o site de demonstração são criados pela primeira vez o  *<a id="_msoanchor_2"> </a> [uma visão geral de formulários de autenticação](../introduction/an-overview-of-forms-authentication-cs.md)*  tutorial e, em seguida, aprimorado no  *<a id="https://www.asp.net/learn/security/tutorial-03-cs.aspx"> </a> configuração de autenticação de formulários e tópicos avançados* tutorial. Nosso aplicativo da web de demonstração tem uma página de logon que valida as credenciais do usuário em pares de nome de usuário/senha embutida. Além disso, `Global.asax` inclui o código que cria personalizado `IPrincipal` e `IIdentity` objetos para usuários autenticados. Atualizaremos a página de logon para validar as credenciais do usuário em relação a estrutura de associação e remova a lógica principal e identidade personalizada.

Vamos começar!

## <a name="the-forms-authentication-and-membership-checklist"></a>A autenticação de formulários e a lista de verificação de associação

Antes de começar a trabalhar com a estrutura de associação, vamos examinar as etapas importantes que levamos para chegar a este ponto. Ao usar a estrutura de associação com o `SqlMembershipProvider` em um cenário de autenticação baseada em formulários, as etapas a seguir precisam ser executadas antes de implementar a funcionalidade de associação em seu aplicativo web:

1. **Habilite a autenticação baseada em formulários.** Conforme abordado em  *<a id="_msoanchor_4"> </a> [uma visão geral de formulários de autenticação](../introduction/an-overview-of-forms-authentication-cs.md)*, autenticação de formulários é habilitada por meio da edição `Web.config` e configuração de `<authentication>` elemento `mode` atributo `Forms`. Com a autenticação de formulários habilitada, cada solicitação de entrada é examinada para um *tíquete de autenticação de formulários*, que, se presente, identifica o solicitante.
2. **Adicione o esquema do serviços de aplicativo para o banco de dados apropriado.** Ao usar o `SqlMembershipProvider` , precisamos instalar o esquema do serviços de aplicativo para um banco de dados. Normalmente esse esquema é adicionado ao mesmo banco de dados que contém o modelo de dados do aplicativo. O  *<a id="_msoanchor_5"> </a> [criar o esquema de associação no SQL Server](creating-the-membership-schema-in-sql-server-cs.md)*  tutorial visto usando o `aspnet_regsql.exe` ferramenta para fazer isso.
3. **Personalize as configurações do aplicativo da Web para o banco de dados de referência da etapa 2.** O *criar o esquema de associação no SQL Server* tutorial mostrou duas maneiras de configurar o aplicativo web para que o `SqlMembershipProvider` usaria o banco de dados selecionado na etapa 2: modificando o `LocalSqlServer` nome de cadeia de caracteres de conexão; ou adicionando um novo provedor registrado para a lista de provedores de estrutura de associação e personalizando a esse novo provedor para usar o banco de dados na etapa 2.

Ao criar um aplicativo web que usa o `SqlMembershipProvider` e autenticação baseada em formulários, você precisará seguir estas três etapas antes de usar o `Membership` classe ou os controles da Web de logon do ASP.NET. Como nós já executou estas etapas nos tutoriais anteriores, você está pronto para começar a usar a estrutura de associação!

## <a name="step-1-adding-new-aspnet-pages"></a>Etapa 1: Adicionar novas páginas ASP.NET

Este tutorial e as próximas três nós serão examinando diversos recursos e funções relacionadas à associação. Precisaremos de uma série de páginas do ASP.NET para implementar os tópicos examinados durante esses tutoriais. Vamos criar as páginas e, em seguida, um arquivo de mapa de site `(Web.sitemap)`.

Comece criando uma nova pasta no projeto chamado `Membership`. Em seguida, adicione cinco novas páginas ASP.NET para o `Membership` vinculação de pastas, cada página com o `Site.master` página mestra. Nomeie as páginas:

- `CreatingUserAccounts.aspx`
- `UserBasedAuthorization.aspx`
- `EnhancedCreateUserWizard.aspx`
- `AdditionalUserInfo.aspx`
- `Guestbook.aspx`

Neste ponto Gerenciador de soluções do projeto deve ser semelhante à mostrada na Figura 1 de captura de tela.


[![Cinco novas páginas foram adicionadas para a pasta de associação](creating-user-accounts-cs/_static/image2.png)](creating-user-accounts-cs/_static/image1.png)

**Figura 1**: cinco novas páginas foram adicionados para o `Membership` pasta ([clique para exibir a imagem em tamanho normal](creating-user-accounts-cs/_static/image3.png))


Cada página neste ponto, deve ter os dois controles de conteúdo, uma para cada ContentPlaceHolders da página mestra: `MainContent` e `LoginContent`.

[!code-aspx[Main](creating-user-accounts-cs/samples/sample1.aspx)]

Lembre-se de que o `LoginContent` marcação de padrão do ContentPlaceHolder exibe um link para fazer logon ou logoff do site, dependendo se o usuário é autenticado. A presença de `Content2` controle de conteúdo, no entanto, substitui a marcação de padrão da página mestra. Conforme abordado em  *<a id="_msoanchor_6"> </a> [uma visão geral de formulários de autenticação](../introduction/an-overview-of-forms-authentication-cs.md)*  tutorial, isso é útil nas páginas onde não desejamos exibir opções relacionadas ao logon na coluna esquerda.

Essas páginas de cinco, no entanto, queremos mostrar uma marcação padrão da página mestra para o `LoginContent` ContentPlaceHolder. Portanto, remova a marcação declarativa para o `Content2` controle de conteúdo. Depois de fazer isso, cada marcação da página cinco deve conter somente um controle de conteúdo.

## <a name="step-2-creating-the-site-map"></a>Etapa 2: Criar o mapa de Site

Tudo, exceto os sites da Web mais trivial precisam implementar alguma forma de uma interface do usuário de navegação. A interface do usuário de navegação pode ser uma lista simple de links para as várias seções do site. Como alternativa, esses links podem ser organizados em menus ou modos de exibição de árvore. Como os desenvolvedores de páginas, criar a interface do usuário de navegação é somente metade da história. Também precisamos de alguma maneira para definir a estrutura lógica do site em um modo de manutenção e atualizável. Conforme novas páginas são adicionadas ou removidos de páginas existentes, você deseja ser capaz de atualizar uma única fonte – o mapa do site – e tem essas modificações refletidas na interface do usuário de navegação do site.

Essas duas tarefas – definir o mapa de site e implementar uma interface do usuário de navegação com base no mapa do site – são fáceis de realizar graças a estrutura de mapa do Site e os controles de navegação Web adicionados no ASP.NET 2.0. A estrutura de mapa de Site permite que um desenvolvedor definir um mapa de site e, em seguida, acessá-lo por meio de uma API programática (o [ `SiteMap` classe](https://msdn.microsoft.com/library/system.web.sitemap.aspx)). Controles da Web de navegação internos incluem um [controle Menu](https://msdn.microsoft.com/library/bz09dy46.aspx), o [controle TreeView](https://msdn.microsoft.com/library/3eafky27.aspx)e o [controle SiteMapPath](https://msdn.microsoft.com/library/3eafky27.aspx).

Como as estruturas de associação e funções, a estrutura de mapa do Site é criada sobre a [modelo de provedor](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx). O trabalho da classe do provedor de mapa do Site é gerar a estrutura na memória usada pelo `SiteMap` classe a partir de um repositório de dados persistentes, como um arquivo XML ou uma tabela de banco de dados. O .NET Framework vem com um provedor de mapa de Site padrão que lê os dados de mapa do site de um arquivo XML ([`XmlSiteMapProvider`](https://msdn.microsoft.com/library/system.web.xmlsitemapprovider.aspx)), e esse é o provedor que vamos usar neste tutorial. Para algumas implementações de provedor de mapa de Site alternativas, consulte a seção Leituras adicionais no final deste tutorial.

O provedor de mapa do Site padrão espera um arquivo XML formatado corretamente chamado `Web.sitemap` existir o diretório raiz. Como estamos usando esse provedor padrão, é preciso adicionar esse arquivo e definir a estrutura do mapa do site no formato XML apropriado. Para adicionar o arquivo, clique no nome do projeto no Gerenciador de soluções e escolha Adicionar Novo Item. Na caixa de diálogo optar por adicionar um arquivo do tipo de mapa de Site denominado `Web.sitemap`.


[![Adicionar um arquivo chamado SiteMap para o diretório raiz do projeto](creating-user-accounts-cs/_static/image5.png)](creating-user-accounts-cs/_static/image4.png)

**Figura 2**: adicionar um arquivo chamado `Web.sitemap` para o diretório raiz do projeto ([clique para exibir a imagem em tamanho normal](creating-user-accounts-cs/_static/image6.png))


O arquivo de mapa de site XML define a estrutura do site como uma hierarquia. Essa relação hierárquica é modelada no arquivo XML por meio de ancestrais do `<siteMapNode>` elementos. O `Web.sitemap` deve começar com uma `<siteMap>` nó pai que tem exatamente um `<siteMapNode>` filho. Nesse nível superior `<siteMapNode>` elemento representa a raiz da hierarquia e pode ter um número arbitrário de nós descendentes. Cada `<siteMapNode>` elemento deve incluir um `title` de atributos e, opcionalmente, pode incluir `url` e `description` atributos, entre outros; cada não-vazia `url` atributo deve ser exclusivo.

Digite o seguinte XML para o `Web.sitemap` arquivo:

[!code-xml[Main](creating-user-accounts-cs/samples/sample2.xml)]

A marcação de mapa de site acima define a hierarquia mostrada na Figura 3.


[![O mapa de Site representa uma estrutura de Navegação hierárquica](creating-user-accounts-cs/_static/image8.png)](creating-user-accounts-cs/_static/image7.png)

**Figura 3**: O mapa de Site representa uma estrutura hierárquica de navegação ([clique para exibir a imagem em tamanho normal](creating-user-accounts-cs/_static/image9.png))


## <a name="step-3-updating-the-master-page-to-include-a-navigational-user-interface"></a>Etapa 3: Atualizar a página mestra para incluir uma Interface do usuário de navegação

O ASP.NET inclui uma série de controles de Web relacionados a navegação para a criação de uma interface do usuário. Isso inclui os controles SiteMapPath, TreeView e Menu. Os controles de Menu e TreeView processam a estrutura de mapa de site em um menu ou uma árvore, respectivamente, enquanto o SiteMapPath exibe uma trilha de navegação que mostra o nó atual está sendo visitado, bem como seus ancestrais. Os dados de mapa de site podem ser associados a outros dados de controles da Web usando o SiteMapDataSource e pode ser acessados por programação via o `SiteMap` classe.

Como uma discussão completa sobre a estrutura de mapa do Site e os controles de navegação está além do escopo esta série de tutoriais, em vez de gastar tempo criar nossa própria interface do usuário de navegação vamos em vez disso, emprestar usada no meu  *[ Trabalhando com dados no ASP.NET 2.0](../../data-access/index.md)*  série de tutoriais, que usa um controle repetidor para exibir uma lista com marcadores dois profundo de links de navegação, como mostrado na Figura 4.

### <a name="adding-a-two-level-list-of-links-in-the-left-column"></a>Adicionando uma lista de dois níveis de Links na coluna esquerda

Para criar esta interface, adicione a seguinte marcação declarativa para o `Site.master` página mestra da coluna esquerda em que o texto "TODO: Menu aparece aqui..." reside no momento.

[!code-aspx[Main](creating-user-accounts-cs/samples/sample3.aspx)]

A marcação acima associa um controle repetidor chamado `menu` a SiteMapDataSource, que retorna a hierarquia de mapa do site definida no `Web.sitemap`. Desde o controle de SiteMapDataSource [ `ShowStartingNode` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sitemapdatasource.showstartingnode.aspx) está definida como False, ele começa a retornar a hierarquia do mapa de site começando com os descendentes do nó "Home". Repetidor exibe cada um de nós (atualmente, apenas "associação") em um `<li>` elemento. Repetidor interna, outro, em seguida, exibe os filhos do nó atual em uma lista não ordenada aninhada.

A Figura 4 mostra a saída renderizada da marcação acima com a estrutura de mapa de site criado na etapa 2. Repetidor renderiza marcações de lista não ordenada de baunilha; as regras de folha de estilo em cascata definidas na `Styles.css` são responsáveis pelo layout estética forma. Para obter uma descrição mais detalhada de como funciona a marcação acima, consulte o [páginas mestras e navegação de Site](https://asp.net/learn/data-access/tutorial-03-cs.aspx) tutorial.


[![A Interface do usuário de navegação é renderizado usando aninhado não ordenada listas](creating-user-accounts-cs/_static/image11.png)](creating-user-accounts-cs/_static/image10.png)

**Figura 4**: A Interface de usuário de navegação é renderizado usando aninhado não ordenada listas ([clique para exibir a imagem em tamanho normal](creating-user-accounts-cs/_static/image12.png))


### <a name="adding-breadcrumb-navigation"></a>Adicionando navegação estrutural

Além da lista de links na coluna esquerda, vamos também ter cada exibição de página um [trilha](http://en.wikipedia.org/wiki/Breadcrumb_%28navigation%29). Uma trilha é um elemento de interface do usuário de navegação que mostra os usuários rapidamente sua posição atual dentro da hierarquia do site. O controle SiteMapPath usa a estrutura de mapa de Site para determinar o local da página atual no mapa do site e, em seguida, exibe uma trilha com base nessas informações.

Especificamente, adicionar um `<span>` elemento ao cabeçalho da página mestra `<div>` elemento e definir a nova `<span>` do elemento `class` atributo "trilha". (O `Styles.css` classe contém uma regra para uma classe "trilha".) Em seguida, adicione um SiteMapPath nesse novo `<span>` elemento.

[!code-aspx[Main](creating-user-accounts-cs/samples/sample4.aspx)]

A Figura 5 mostra a saída de SiteMapPath ao visitar `~/Membership/CreatingUserAccounts.aspx`.


[![A trilha de navegação exibe a página atual e mapeiam seus ancestrais no Site](creating-user-accounts-cs/_static/image14.png)](creating-user-accounts-cs/_static/image13.png)

**Figura 5**: navegação estrutural exibe a página atual e seus ancestrais no mapa do Site ([clique para exibir a imagem em tamanho normal](creating-user-accounts-cs/_static/image15.png))


## <a name="step-4-removing-the-custom-principal-and-identity-logic"></a>Etapa 4: Remover a entidade de segurança personalizada e a lógica de identidade

No  *<a id="_msoanchor_7"> </a> [configuração de autenticação de formulários e tópicos avançados](../introduction/forms-authentication-configuration-and-advanced-topics-cs.md)*  tutorial vimos como associar objetos principal e identidade personalizados para o usuário autenticado. Conseguimos isso criando um manipulador de eventos em `Global.asax` para o aplicativo `PostAuthenticateRequest` evento, que é acionado depois que o `FormsAuthenticationModule` autenticou o usuário. Este manipulador de eventos substituímos o `GenericPrincipal` e `FormsIdentity` objetos adicionados pelo `FormsAuthenticationModule` com o `CustomPrincipal` e `CustomIdentity` objetos criado neste tutorial.

Enquanto os objetos principal e identidade personalizados são úteis em determinados cenários, na maioria dos casos o `GenericPrincipal` e `FormsIdentity` objetos são suficientes. Consequentemente, acho que seria útil para retornar para o comportamento padrão. Fazer essa alteração, removendo ou comentando o `PostAuthenticateRequest` manipulador de eventos ou excluindo o `Global.asax` totalmente.

## <a name="step-5-programmatically-creating-a-new-user"></a>Etapa 5: Criando um novo usuário programaticamente

Para criar uma nova conta de usuário por meio do uso de estrutura de associação a `Membership` da classe [ `CreateUser` método](https://msdn.microsoft.com/library/system.web.security.membership.createuser.aspx). Esse método tem parâmetros para o nome de usuário, senha e outros campos relacionados ao usuário de entrada. Na invocação, delega a criação da nova conta de usuário para o provedor de associação configurado e, em seguida, retorna um [ `MembershipUser` objeto](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx) que representa a conta de usuário recém-criada.

O `CreateUser` método tem quatro sobrecargas, cada aceitar um número diferente de parâmetros de entrada:

- [`CreateUser(username, password)`](https://msdn.microsoft.com/library/d8t4h2es.aspx)
- [`CreateUser(username, password, email)`](https://msdn.microsoft.com/library/t8yy6w3h.aspx)
- [`CreateUser(username, password, email, passwordQuestion, passwordAnswer, isApproved, MembershipCreateStatus)`](https://msdn.microsoft.com/library/82xx2e62.aspx)
- [`CreateUser(username, password, email, passwordQuestion, passwordAnswer, isApproved, providerUserKey, MembershipCreateStatus)`](https://msdn.microsoft.com/library/ms152012.aspx)

Essas quatro sobrecargas diferem na quantidade de informações coletadas. A primeira sobrecarga, por exemplo, requer apenas o nome de usuário e a senha para a nova conta de usuário, enquanto o outro também requer o endereço de email do usuário.

Essas sobrecargas existem porque as informações necessárias para criar uma nova conta de usuário dependem de definições de configuração do provedor de associação. No  *<a id="_msoanchor_8"> </a> [criar o esquema de associação no SQL Server](creating-the-membership-schema-in-sql-server-cs.md)*  tutorial examinamos especifica definições de configuração de provedor de associação no `Web.config`. Tabela 2 incluída uma lista completa das definições de configuração.

Uma configuração de provedor tal associação configuração afeta o que `CreateUser` sobrecargas podem ser usadas é o `requiresQuestionAndAnswer` configuração. Se `requiresQuestionAndAnswer` é definido como `true` (o padrão), em seguida, ao criar uma nova conta de usuário, deve especificar uma pergunta de segurança e a resposta. Essas informações são usadas mais tarde se o usuário precisar redefinir ou alterar sua senha. Especificamente, nesse momento, elas serão mostradas a pergunta de segurança e eles devem digitar a resposta correta para redefinir ou alterar sua senha. Consequentemente, se o `requiresQuestionAndAnswer` é definido como `true` , em seguida, chamar qualquer uma das duas primeiras `CreateUser` sobrecargas resulta em uma exceção porque a pergunta de segurança e a resposta estão ausentes. Como o nosso aplicativo está atualmente configurado para exigir uma pergunta de segurança e uma resposta, precisamos usar uma das duas sobrecargas último durante a criação do usuário por meio de programação.

Para ilustrar o uso de `CreateUser` método, vamos criar uma interface do usuário em que podemos solicitar ao usuário seu nome, senha, email e uma resposta a uma pergunta de segurança predefinidos. Abra o `CreatingUserAccounts.aspx` página o `Membership` pasta e adicione os seguintes controles da Web para o controle de conteúdo:

- Uma caixa de texto denominada`Username`
- Uma caixa de texto denominada `Password`, cujo `TextMode` está definida como`Password`
- Uma caixa de texto denominada`Email`
- Um rótulo denominado `SecurityQuestion` com seus `Text` propriedade limpo
- Uma caixa de texto denominada`SecurityAnswer`
- Um botão chamado `CreateAccountButton` cuja propriedade de texto é definida como "Criar a conta de usuário"
- Um controle de rótulo nomeado `CreateAccountResults` com seus `Text` propriedade limpo

Agora sua tela deve ser semelhante à mostrada na Figura 6 de captura de tela.


[![Adicionar vários controles da Web para a página CreatingUserAccounts.aspx](creating-user-accounts-cs/_static/image17.png)](creating-user-accounts-cs/_static/image16.png)

**Figura 6**: adicionar vários controles de Web para o `CreatingUserAccounts.aspx` página ([clique para exibir a imagem em tamanho normal](creating-user-accounts-cs/_static/image18.png))


O `SecurityQuestion` rótulo e `SecurityAnswer` TextBox destinam-se para exibir uma pergunta de segurança predefinidos e coletar a resposta do usuário. Observe que a pergunta de segurança e de resposta são armazenados em uma base por usuário, portanto, é possível permitir que cada usuário definir seu próprios pergunta de segurança. No entanto, para este exemplo, decidi usar uma pergunta de segurança universal, isto é: "Qual é a sua cor favorita?"

Para implementar essa pergunta de segurança predefinidos, adicione uma constante para a classe de code-behind da página chamada `passwordQuestion`, atribuí-lo a pergunta de segurança. Em seguida, no `Page_Load` manipulador de eventos, atribuir nesse constante para o `SecurityQuestion` do rótulo `Text` propriedade:

[!code-csharp[Main](creating-user-accounts-cs/samples/sample5.cs)]

Em seguida, crie um manipulador de eventos para o `CreateAccountButton`do `Click` evento e adicione o seguinte código:

[!code-csharp[Main](creating-user-accounts-cs/samples/sample6.cs)]

O `Click` manipulador de eventos é iniciado, definindo uma variável chamada `createStatus` do tipo [ `MembershipCreateStatus` ](https://msdn.microsoft.com/library/system.web.security.membershipcreatestatus.aspx). `MembershipCreateStatus`é uma enumeração que indica o status do `CreateUser` operação. Por exemplo, se a conta de usuário é criada com êxito, o resultante `MembershipCreateStatus` instância será definida como um valor de `Success`; no outro lado, se a operação falhar porque já existe um usuário com o mesmo nome de usuário, ele será definido como um valor de `DuplicateUserName`. No `CreateUser` sobrecarga usamos, precisamos passar um `MembershipCreateStatus` instância para o método como um `out` parâmetro. Esse parâmetro é definido como o valor apropriado dentro do `CreateUser` método e vamos examinar seu valor após a chamada de método para determinar se a conta de usuário foi criada com êxito.

Depois de chamar `CreateUser`, passando `createStatus`, um `switch` instrução é usada para produzir uma mensagem apropriada dependendo do valor atribuído a `createStatus`. Figuras 7 mostra a saída quando um novo usuário foi criado com êxito. Figuras 8 e 9 mostram a saída quando a conta de usuário não é criada. Na Figura 8, o visitante inserido uma senha de letra de cinco, que não atende aos requisitos de força de senha previstos em definições de configuração do provedor de associação. Na Figura 9, o visitante está tentando criar uma conta de usuário com um nome de usuário existente (aquele criado na Figura 7).


[![Uma nova conta de usuário é criado com êxito](creating-user-accounts-cs/_static/image20.png)](creating-user-accounts-cs/_static/image19.png)

**Figura 7**: A nova conta de usuário é criado com êxito ([clique para exibir a imagem em tamanho normal](creating-user-accounts-cs/_static/image21.png))


[![A conta de usuário não é criada porque a senha fornecida é muito fraca](creating-user-accounts-cs/_static/image23.png)](creating-user-accounts-cs/_static/image22.png)

**Figura 8**: A conta de usuário não é criada porque a senha fornecida é muito fraca ([clique para exibir a imagem em tamanho normal](creating-user-accounts-cs/_static/image24.png))


[![A conta de usuário não é criado porque o nome de usuário já em uso](creating-user-accounts-cs/_static/image26.png)](creating-user-accounts-cs/_static/image25.png)

**Figura 9**: A conta de usuário não é criado porque o nome de usuário já em uso ([clique para exibir a imagem em tamanho normal](creating-user-accounts-cs/_static/image27.png))


> [!NOTE]
> Você deve estar se perguntando como determinar o êxito ou Falha ao usar uma das duas primeiras `CreateUser` sobrecargas do método, nenhum deles tem um parâmetro do tipo `MembershipCreateStatus`. Essas duas primeiras sobrecargas lançam um [ `MembershipCreateUserException` exceção](https://msdn.microsoft.com/library/system.web.security.membershipcreateuserexception.aspx) no caso de uma falha, que inclui um [ `StatusCode` propriedade](https://msdn.microsoft.com/library/system.web.security.membershipcreateuserexception.statuscode.aspx) do tipo `MembershipCreateStatus`.


Depois de criar algumas contas de usuário, verifique se as contas foram criadas pela listagem do conteúdo do `aspnet_Users` e `aspnet_Membership` tabelas no `SecurityTutorials.mdf` banco de dados. Como mostra a Figura 10, adicionei dois usuários por meio de `CreatingUserAccounts.aspx` página: Tito e Bruce.


[![Há dois usuários no repositório do usuário de associação: Tito e Bruce](creating-user-accounts-cs/_static/image29.png)](creating-user-accounts-cs/_static/image28.png)

**Figura 10**: há dois usuários no repositório do usuário de associação: Tito e Bruce ([clique para exibir a imagem em tamanho normal](creating-user-accounts-cs/_static/image30.png))


Enquanto o repositório de usuário de associação agora inclui informações de conta Bruce e de Tito, ainda precisamos implementar a funcionalidade que permite Bruce ou Tito fazer logon no site. Atualmente, `Login.aspx` valida as credenciais do usuário em um conjunto de pares de nome de usuário/senha – de embutida faz *não* validar as credenciais fornecidas no Framework de associação. Para ver agora novas contas de usuário no `aspnet_Users` e `aspnet_Membership` tabelas devem ser suficientes. No tutorial de Avançar,  *<a id="_msoanchor_9"> </a> [validar o usuário credenciais em relação a associação usuário armazenar](validating-user-credentials-against-the-membership-user-store-cs.md)*, atualizaremos a página de logon para validar em relação ao armazenamento de associação.

> [!NOTE]
> Se você não vir todos os usuários em sua `SecurityTutorials.mdf` banco de dados, é possível que seu aplicativo web é usando o provedor de associação padrão, `AspNetSqlMembershipProvider`, que usa o `ASPNETDB.mdf` banco de dados como seu repositório do usuário. Para determinar se esse é o problema, clique no botão Atualizar no Gerenciador de soluções. Se um banco de dados denominado `ASPNETDB.mdf` foi adicionado para o `App_Data` pasta, esse é o problema. Retornar à etapa 4 do  *<a id="_msoanchor_10"> </a> [criar o esquema de associação no SQL Server](creating-the-membership-schema-in-sql-server-cs.md)*  tutorial para obter instruções sobre como configurar corretamente o provedor de associação.


Na maioria dos criar usuário cenários de conta, o visitante é apresentado com alguma interface para inserir seu nome de usuário, senha, email e outras informações essenciais, no ponto em que é criada uma nova conta. Nesta etapa, examinamos a criação de tal interface manualmente e, em seguida, vimos como usar o `Membership.CreateUser` método para adicionar programaticamente a nova conta de usuário com base nas entradas do usuário. Nosso código, no entanto, acabou de criar a nova conta de usuário. Ele não executou qualquer acompanhamento das ações, como fazer logon do usuário para o site sob a conta de usuário recém-criada ou enviando um email de confirmação para o usuário. Estas etapas adicionais exigiria código adicional no botão de `Click` manipulador de eventos.

O ASP.NET é fornecido com o controle CreateUserWizard, que é projetado para lidar com o processo de criação de conta de usuário, de uma interface de usuário para criar novas contas de usuário, para criar a conta na estrutura de associação e executar pós-conta de processamento tarefas de criação, como enviar um email de confirmação e registro de usuário recém-criada no site. Usar o controle CreateUserWizard é tão simple quanto arrastando o controle CreateUserWizard da caixa de ferramentas em uma página e, em seguida, definir algumas propriedades. Na maioria dos casos, você não precisa escrever uma única linha de código. Iremos explorar esse controle excelente em detalhes na etapa 6.

Se as novas contas de usuário só são criadas por meio de uma página da web de criar uma conta comum, é improvável que você alguma vez precisar escrever código que usa o `CreateUser` método, como o controle CreateUserWizard provavelmente atenda às suas necessidades. No entanto, o `CreateUser` método é útil em cenários em que você precisa de uma experiência de usuário de criar uma conta altamente personalizada ou quando você precisa criar programaticamente novas contas de usuário por meio de uma interface alternativa. Por exemplo, você pode ter uma página que permite que um usuário carregar um arquivo XML que contém informações de usuário de algum outro aplicativo. A página pode analisa o conteúdo do XML carregado do arquivo e cria uma nova conta para cada usuário representado no XML, chamando o `CreateUser` método.

## <a name="step-6-creating-a-new-user-with-the-createuserwizard-control"></a>Etapa 6: Criando um novo usuário com o controle CreateUserWizard

ASP.NET vem com um número de controles da Web de logon. Esses controles ajudam em muitos cenários comuns de usuário relacionadas a conta e logon. O [controle CreateUserWizard](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/createuserwizard.aspx) esse controle que é projetado para apresentar uma interface do usuário para adicionar uma nova conta de usuário para a estrutura de associação.

Como muitos dos outros controles da Web relacionadas a logon, CreateUserWizard pode ser usado sem escrever uma única linha de código. Intuitivamente fornece uma interface de usuário com base em parâmetros de configuração do provedor de associação e internamente chama o `Membership` da classe `CreateUser` método depois que o usuário insere as informações necessárias e clica no botão "Criar usuário". O controle CreateUserWizard é extremamente personalizável. Há uma série de eventos acionados durante vários estágios do processo de criação de conta. Podemos criar manipuladores de eventos, conforme necessário, para injetar lógica personalizada para o fluxo de trabalho de criação de conta. Além disso, a aparência do CreateUserWizard é muito flexível. Há um número de propriedades que definem a aparência da interface padrão; Se necessário, o controle pode ser convertido em um modelo ou o registro de usuário adicionais "etapas" pode ser adicionado.

Vamos começar com uma olhada usando a interface padrão e o comportamento do controle CreateUserWizard. Em seguida, exploraremos como personalizar a aparência por meio de eventos e propriedades do controle.

### <a name="examining-the-createuserwizards-default-interface-and-behavior"></a>Examinando a Interface padrão e o comportamento do CreateUserWizard

Volte para o `CreatingUserAccounts.aspx` página o `Membership` pasta, alterne para o modo de Design ou de divisão e, em seguida, adicionar um controle CreateUserWizard à parte superior da página. O controle CreateUserWizard é arquivado na seção de controles de logon da caixa de ferramentas. Depois de adicionar o controle, defina seu `ID` propriedade `RegisterUser`. Como a captura de tela na Figura 11 mostra, CreateUserWizard renderiza uma interface com caixas de texto para o novo nome de usuário, senha, endereço de email e pergunta de segurança e resposta.


[![Controle CreateUserWizard processa um genérico cria Interface do usuário](creating-user-accounts-cs/_static/image32.png)](creating-user-accounts-cs/_static/image31.png)

**Figura 11**: O controle CreateUserWizard renderiza uma Interface de usuário genérica criar ([clique para exibir a imagem em tamanho normal](creating-user-accounts-cs/_static/image33.png))


Vamos dedicar um tempo para comparar a interface do usuário padrão gerada pelo controle CreateUserWizard com a interface que criamos na etapa 5. Para começar, o controle CreateUserWizard permite que o visitante especificar a pergunta de segurança e a resposta, enquanto a nossa interface criados manualmente usada uma pergunta de segurança predefinidos. Interface do controle CreateUserWizard também inclui controles de validação, enquanto ainda precisamos implementar a validação em campos do formulário da nossa interface. E a interface do controle CreateUserWizard inclui uma caixa de texto "Confirmar senha" (junto com um CompareValidator para garantir que o texto digitado "Senha" e "Senha comparar" caixas de texto são iguais).

O interessante é que o controle CreateUserWizard consulta definições de configuração do provedor de associação durante a renderização de sua interface do usuário. Por exemplo, as caixas de texto de pergunta e resposta de segurança são exibidas somente se `requiresQuestionAndAnswer` for definido como True. Da mesma forma, CreateUserWizard adiciona automaticamente um controle RegularExpressionValidator para garantir que os requisitos de força de senha são atendidos e define seu `ErrorMessage` e `ValidationExpression` propriedades como base o `minRequiredPasswordLength`, `minRequiredNonalphanumericCharacters`e `passwordStrengthRegularExpression` definições de configuração.

O controle CreateUserWizard, como o nome sugere, é derivado de [controle Wizard](https://msdn.microsoft.com/library/s2etd1ek.aspx). Controles de assistente são projetados para fornecer uma interface para concluir as tarefas de várias etapas. Um controle Wizard pode ter um número arbitrário de `WizardSteps`, cada um deles é um modelo que define o HTML e controles da Web para essa etapa. O controle Wizard inicialmente exibe a primeira `WizardStep`, juntamente com os controles de navegação que permitem que o usuário para passar de uma etapa para a próxima ou para retornar às etapas anteriores.

Como mostra a marcação declarativa na Figura 11, a interface de padrão de controle CreateUserWizard inclui dois`WizardSteps:`

- [`CreateUserWizardStep`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizardstep.aspx)– processa a interface para coletar informações sobre como criar a nova conta de usuário. Esta é a etapa mostrada na Figura 11.
- [`CompleteWizardStep`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.completewizardstep.aspx)– processa uma mensagem indicando que a conta foi criada corretamente.

O CreateUserWizard aparência e comportamento podem ser modificados por converter qualquer uma das seguintes etapas para modelos, ou adicionando seus próprios `WizardSteps`. Examinaremos adicionando um `WizardStep` na interface de registro no *armazenar informações de usuário adicionais* tutorial.

Vamos ver o controle CreateUserWizard em ação. Visite o `CreatingUserAccounts.aspx` página através de um navegador. Inicie inserindo alguns valores inválidos na interface do CreateUserWizard. Tente inserindo uma senha que não está em conformidade com os requisitos de força de senha ou deixar o "nome de usuário" caixa de texto vazia. CreateUserWizard exibirá uma mensagem de erro apropriado. Figura 12 mostra a saída ao tentar criar um usuário com uma senha forte insuficiente.


[![CreateUserWizard injeta automaticamente os controles de validação](creating-user-accounts-cs/_static/image35.png)](creating-user-accounts-cs/_static/image34.png)

**Figura 12**: O CreateUserWizard automaticamente injeta controles de validação ([clique para exibir a imagem em tamanho normal](creating-user-accounts-cs/_static/image36.png))


Em seguida, insira os valores apropriados em CreateUserWizard e clique no botão "Criar usuário". Supondo que os campos obrigatórios foram inseridos e força da senha é suficiente, CreateUserWizard criará uma nova conta de usuário por meio da estrutura de associação e, em seguida, exibir o `CompleteWizardStep`da interface (consulte a Figura 13). Nos bastidores, CreateUserWizard chama o `Membership.CreateUser` método, como na etapa 5.


[![Uma nova conta de usuário tiver sido criado com êxito](creating-user-accounts-cs/_static/image38.png)](creating-user-accounts-cs/_static/image37.png)

**Figura 13**: A nova conta de usuário tem foi criado com êxito ([clique para exibir a imagem em tamanho normal](creating-user-accounts-cs/_static/image39.png))


> [!NOTE]
> Como mostra a Figura 13, o `CompleteWizardStep`da interface inclui um botão de continuação. No entanto, neste momento clicando nele simplesmente executa um postback, deixando o visitante na mesma página. Na seção "Personalizando o CreateUserWizard aparência e comportamento através de suas propriedades" veremos como você pode ter esse botão enviar o visitante `Default.aspx` (ou alguma outra página).


Depois de criar uma nova conta de usuário, retorne ao Visual Studio e examine o `aspnet_Users` e `aspnet_Membership` tabelas como fizemos na Figura 10 para verificar se a conta foi criada com êxito.

### <a name="customizing-the-createuserwizards-behavior-and-appearance-through-its-properties"></a>Personalizando o CreateUserWizard comportamento e aparência através de suas propriedades

CreateUserWizard pode ser personalizado de várias maneiras, por meio de propriedades, `WizardSteps`e manipuladores de eventos. Nesta seção examinaremos como personalizar a aparência do controle por meio de suas propriedades. a próxima seção examina estender o comportamento do controle por meio de manipuladores de eventos.

Praticamente todo o texto exibido na interface do usuário padrão do controle CreateUserWizard pode ser personalizado por meio de seu grande quantidade de propriedades. Por exemplo, os rótulos de "Nome de usuário", "Senha", "Confirmar senha", "Email", "Pergunta de segurança" e "Resposta de segurança" exibidos à esquerda das caixas de texto podem ser personalizados com o [ `UserNameLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.usernamelabeltext.aspx), [ `PasswordLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.passwordlabeltext.aspx), [ `ConfirmPasswordLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.confirmpasswordlabeltext.aspx), [ `EmailLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.emaillabeltext.aspx), [ `QuestionLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.questionlabeltext.aspx), e [ `AnswerLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.answerlabeltext.aspx)propriedades, respectivamente. Da mesma forma, há propriedades para especificar o texto para os botões "Create User" e "Continue" o `CreateUserWizardStep` e `CompleteWizardStep`, bem como se esses botões são renderizados como botões, botões de link ou ImageButtons.

As cores, bordas, fontes e outros elementos visuais são configuráveis por meio de um host de propriedades de estilo. O próprio controle CreateUserWizard tem as propriedades de estilo de controle da Web comuns – `BackColor`, `BorderStyle`, `CssClass`, `Font`e assim por diante – e há um número de propriedades de estilo para definir a aparência de seções específicas das Interface do CreateUserWizard. O [ `TextBoxStyle` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.textboxstyle.aspx), por exemplo, define os estilos de caixas de texto no `CreateUserWizardStep`, enquanto o [ `TitleTextStyle` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.titletextstyle.aspx) define o estilo do título ("inscrever-se para o novo Conta").

Além das propriedades relacionadas a aparência, há um número de propriedades que afetam o comportamento do controle CreateUserWizard. O [ `DisplayCancelButton` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.wizard.displaycancelbutton.aspx), se definido como True, exibe um botão de cancelamento ao lado do botão "Criar usuário" (o valor padrão é falso). Se você exibir o botão Cancelar, certifique-se de também definir o [ `CancelDestinationPageUrl` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.continuedestinationpageurl.aspx), que especifica a página que o usuário é enviado para depois de clicar em Cancelar. Conforme observado na seção anterior, o botão Continuar no `CompleteWizardStep`da interface causa um postback, mas deixa o visitante na mesma página. Para enviar o visitante a alguma outra página depois de clicar no botão para continuar, basta especificar a URL de [ `ContinueDestinationPageUrl` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.continuedestinationpageurl.aspx).

Vamos atualizar o `RegisterUser` controle CreateUserWizard para mostrar um botão de cancelamento e enviar o visitante `Default.aspx` quando clicar no botão Cancelar ou continuar. Para fazer isso, defina o `DisplayCancelButton` propriedade como True e ambas as `CancelDestinationPageUrl` e `ContinueDestinationPageUrl` propriedades para "~ / default. aspx". A Figura 14 mostra CreateUserWizard atualizado quando visualizada através de um navegador.


[![O CreateUserWizardStep inclui um botão de cancelamento](creating-user-accounts-cs/_static/image41.png)](creating-user-accounts-cs/_static/image40.png)

**Figura 14**: O `CreateUserWizardStep` inclui um botão Cancelar ([clique para exibir a imagem em tamanho normal](creating-user-accounts-cs/_static/image42.png))


Quando um visitante entra em um nome de usuário, senha, endereço de email e pergunta de segurança e resposta e clicar em "Create User", é criada uma nova conta de usuário e o visitante está conectado como usuário recém-criado. Supondo que a pessoa que está visitando a página é criar uma nova conta para si mesmos, isso provavelmente é o comportamento desejado. No entanto, você talvez queira permitir que os administradores adicionar novas contas de usuário. Dessa forma, seria possível criar a conta de usuário, mas o administrador permanecer conectado como administrador (e não como a conta criada recentemente). Esse comportamento pode ser modificado por meio de booliano [ `LoginCreatedUser` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.logincreateduser.aspx).

Contas de usuário na estrutura de associação contêm um sinalizador aprovado; os usuários que não foram aprovados não conseguem fazer login no site. Por padrão, uma nova conta criada está marcada como aprovado, permitindo que o usuário faça logon no site imediatamente. No entanto, é possível ter novas contas de usuário marcadas como não aprovados. Talvez você queira que um administrador aprovar manualmente os novos usuários antes de fazer logon. ou talvez você queira verificar se o endereço de email inserido na inscrição é válido antes de permitir que um usuário faça logon. Tudo o que pode ser o caso que a conta de usuário recém-criada marcada como não aprovado pela definição do controle CreateUserWizard [ `DisableCreatedUser` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.disablecreateduser.aspx) como True (o padrão é falso).

Incluem outras propriedades relacionadas a comportamento de anotação `AutoGeneratePassword` e `MailDefinition`. Se o [ `AutoGeneratePassword` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.autogeneratepassword.aspx) está definida como True, o `CreateUserWizardStep` não exibirá caixas de texto 'Senha' e 'Confirmar senha'; em vez disso, senha do usuário recém-criado é gerada automaticamente usando o `Membership` classe [ `GeneratePassword` método](https://msdn.microsoft.com/library/system.web.security.membership.generatepassword.aspx). O `GeneratePassword` método constrói uma senha de um comprimento especificado e com um número suficiente de caracteres não alfanuméricos para satisfazer os requisitos de força de senha configurada.

O [ `MailDefinition` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.maildefinition.aspx) será útil se você deseja enviar um email para o endereço de email especificado durante o processo de criação de conta. O `MailDefinition` propriedade inclui uma série de subpropriedades para definir informações sobre a mensagem de email construído. Esses subpropriedades incluem opções como `Subject`, `Priority`, `IsBodyHtml`, `From`, `CC`, e `BodyFileName`. O [ `BodyFileName` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.bodyfilename.aspx) aponta para um arquivo de texto ou HTML que contém o corpo da mensagem de email. O corpo dá suporte a dois espaços reservados de predefinidos: `<%UserName%>` e `<%Password%>`. Esses espaços reservados, se estiver presente no `BodyFileName` de arquivos, será substituído pelo nome e a senha de usuário recém-criada.

> [!NOTE]
> O `CreateUserWizard` do controle `MailDefinition` propriedade especifica apenas os detalhes sobre a mensagem de email é enviado quando uma nova conta é criada. Ele não inclui todos os detalhes sobre como a mensagem de email enviada (ou seja, se um diretório de servidor ou o recebimento de email SMTP é usado, informações de autenticação e assim por diante). Esses detalhes de baixo nível precisam ser definidos no `<system.net>` seção `Web.config`. Para obter mais informações sobre essas definições de configuração e enviar email de ASP.NET 2.0, em geral, consulte o [perguntas frequentes em SystemNetMail.com](http://www.systemnetmail.com/) e meu artigo [enviar o Email no ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx).


### <a name="extending-the-createuserwizards-behavior-using-event-handlers"></a>Estendendo o comportamento do CreateUserWizard usando manipuladores de eventos

O controle CreateUserWizard eleva um número de eventos durante o fluxo de trabalho. Por exemplo, depois que um visitante insere seu nome de usuário, senha e outras informações pertinentes e clica no botão "Criar usuário", o controle CreateUserWizard gera seu [ `CreatingUser` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.creatinguser.aspx). Se houver um problema durante o processo de criação, o [ `CreateUserError` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createusererror.aspx) é acionado; no entanto, se o usuário é criado com êxito, o [ `CreatedUser` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx) é gerado. Eventos de controle CreateUserWizard adicional que é gerado, mas estes são três os mais germane.

Em determinados cenários, desejamos explorar o fluxo de trabalho CreateUserWizard que podemos fazer com a criação de um manipulador de eventos para o evento apropriado. Para ilustrar isso, vamos aprimorar o `RegisterUser` controle CreateUserWizard para incluir alguma validação personalizada de nome de usuário e senha. Em particular, vamos aprimorar nosso CreateUserWizard para que os nomes de usuário não podem conter espaços à esquerda ou à direita e o nome de usuário não pode aparecer em qualquer lugar na senha. Resumindo, gostaríamos de impedir que alguém criar um nome de usuário como "Scott" ou ter uma combinação de nome de usuário e senha como "Scott" e "Scott.1234".

Para fazer isso, vamos criar um manipulador de eventos para o `CreatingUser` evento para executar o nosso verificações de validação adicional. Se os dados fornecidos não são válidos é necessário cancelar o processo de criação. Também precisamos adicionar um controle de Web de rótulo para a página para exibir uma mensagem explicando que o nome de usuário ou a senha é inválida. Comece adicionando um controle de rótulo sob o controle CreateUserWizard, definindo seu `ID` propriedade `InvalidUserNameOrPasswordMessage` e sua `ForeColor` propriedade `Red`. Limpar seu `Text` propriedade e defina seu `EnableViewState` e `Visible` propriedades como False.

[!code-aspx[Main](creating-user-accounts-cs/samples/sample7.aspx)]

Em seguida, crie um manipulador de eventos do controle CreateUserWizard `CreatingUser` eventos. Para criar um manipulador de eventos, selecione o controle no Designer e, em seguida, vá para a janela de propriedades. A partir daí, clique no ícone de raio a e, em seguida, clique duas vezes no evento apropriado para criar o manipulador de eventos.

Adicione o seguinte código ao manipulador de eventos do `CreatingUser`:

[!code-csharp[Main](creating-user-accounts-cs/samples/sample8.cs)]

Observe que o nome de usuário e senha inseridos no controle CreateUserWizard estão disponíveis por meio de seu [ `UserName` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.username.aspx) e [ `Password` propriedades](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.password.aspx), respectivamente. Usamos essas propriedades no manipulador de eventos anteriores para determinar se o nome de usuário fornecido contém espaços à esquerda ou à direita e se o nome de usuário foi encontrada dentro a senha. Se alguma dessas condições for atendida, uma mensagem de erro é exibida no `InvalidUserNameOrPasswordMessage` rótulo e o manipulador de eventos `e.Cancel` está definida como `true`. Se `e.Cancel` é definido como `true`, CreateUserWizard reduz seu fluxo de trabalho, efetivamente, cancelar o processo de criação de conta de usuário.

Figura 15 mostra uma captura de tela de `CreatingUserAccounts.aspx` quando o usuário insere um nome de usuário com espaços à esquerda.


[![Nomes de usuário com à esquerda ou espaços à direita não são permitidos](creating-user-accounts-cs/_static/image44.png)](creating-user-accounts-cs/_static/image43.png)

**Figura 15**: nomes de usuário com à esquerda ou espaços à direita não são permitidos ([clique para exibir a imagem em tamanho normal](creating-user-accounts-cs/_static/image45.png))


> [!NOTE]
> Veremos um exemplo do uso do controle CreateUserWizard `CreatedUser` evento o  *<a id="_msoanchor_11"> </a> [armazenar informações de usuário adicionais](storing-additional-user-information-cs.md)*  tutorial.


## <a name="summary"></a>Resumo

O `Membership` da classe `CreateUser` método cria uma nova conta de usuário na estrutura de associação. Ele faz isso delegando a chamada para o provedor de associação configurado. No caso do `SqlMembershipProvider`, o `CreateUser` método adiciona um registro para o `aspnet_Users` e `aspnet_Membership` tabelas de banco de dados.

Enquanto as novas contas de usuário podem ser criadas programaticamente (conforme vimos na etapa 5), uma abordagem mais rápida e mais fácil é usar o controle CreateUserWizard. Esse controle apresenta uma interface do usuário de várias etapas para coletar informações do usuário e criar um novo usuário na estrutura de associação. Nos bastidores, esse controle usa as mesmas `Membership.CreateUser` método como examinada na etapa 5, mas o controle cria a interface do usuário, controles de validação e responde a erros de criação de conta de usuário sem precisar escrever uma linha de código.

Neste ponto, temos a funcionalidade em vigor para criar novas contas de usuário. No entanto, a página de logon ainda está sendo validada em relação a essas credenciais embutido que são especificados no segundo tutorial. No <a id="_msoanchor_12"> </a> [tutorial próximo](validating-user-credentials-against-the-membership-user-store-cs.md) atualizaremos `Login.aspx` validar o usuário do fornecido as credenciais em relação a estrutura de associação.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [`CreateUser`Documentação técnica](https://msdn.microsoft.com/library/system.web.security.membershipprovider.createuser.aspx)
- [Visão geral do controle CreateUserWizard](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/createuserwizard.aspx)
- [Criando um provedor de mapa de Site com base no sistema de arquivos](http://aspnet.4guysfromrolla.com/articles/020106-1.aspx)
- [Criando uma Interface de usuário passo a passo com o controle do ASP.NET 2.0 de Assistente](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx)
- [Examinar o ASP.NET 2.0 da navegação do Site](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Páginas mestras e navegação de Site](https://asp.net/learn/data-access/tutorial-03-vb.aspx)
- [O provedor de mapa de Site do SQL que você estava esperando](https://msdn.microsoft.com/msdnmag/issues/06/02/WickedCode/default.aspx)

### <a name="about-the-author"></a>Sobre o autor

Scott Mitchell, autor de vários livros sobre ASP/ASP.NET e fundador da 4GuysFromRolla. com, trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é  *[Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott pode ser contatado pelo [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) ou em seu blog [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimentos especiais a...

Esta série de tutoriais foi revisado por vários revisores úteis. Revisor levar para este tutorial foi Teresa Murphy. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha no [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

>[!div class="step-by-step"]
[Anterior](creating-the-membership-schema-in-sql-server-cs.md)
[Próximo](validating-user-credentials-against-the-membership-user-store-cs.md)
