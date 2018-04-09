---
uid: identity/overview/getting-started/introduction-to-aspnet-identity
title: Introdução ao ASP.NET identidade | Microsoft Docs
author: jongalloway
description: O sistema de associação do ASP.NET foi introduzido com o ASP.NET 2.0 novamente em 2005 e desde então ter havido muitas alterações de typicall de aplicativos web maneiras...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 38717fc1-5989-43cf-952d-4007cc1dd923
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/getting-started/introduction-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 59272f4659256e108ee99b22eb3bd3e2583a617c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="introduction-to-aspnet-identity"></a>Introdução a identidade do ASP.NET
====================
por [Jon Galloway](https://github.com/jongalloway), [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

> O sistema de associação do ASP.NET foi introduzido com o ASP.NET 2.0 novamente em 2005 e desde então ter havido muitas alterações das maneiras de aplicativos da web geralmente cuida da autenticação e autorização. Identidade do ASP.NET é uma nova aparência no qual o sistema de associação deve ser durante a criação de aplicativos modernos para a web, telefone ou tablet.
> 
> Este artigo foi escrito por Pranav Rastogi ([@rustd](https://twitter.com/rustd)), Jon Galloway ([@jongalloway](https://twitter.com/jongalloway)), Tom Dykstra e Rick Anderson ([ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ).


## <a name="background-membership-in-aspnet"></a>Plano de fundo: Associação ASP.NET

### <a name="aspnet-membership"></a>Associação do ASP.NET

[Associação ASP.NET](https://msdn.microsoft.com/library/yh26yfzy(v=VS.100).aspx) foi projetado para resolver os requisitos de associação de site que eram comuns em 2005, que envolvia autenticação de formulários e um banco de dados do SQL Server para nomes de usuário, senhas e dados de perfil. Atualmente, há uma matriz muito mais ampla de opções de armazenamento de dados para aplicativos web e a maioria dos desenvolvedores deseja habilitar seus sites usar provedores de identidade de redes sociais para a funcionalidade de autenticação e autorização. As limitações de design da associação ASP.NET dificultam essa transição:

- O esquema de banco de dados foi projetado para o SQL Server e você não pode alterá-lo. Você pode adicionar informações de perfil, mas os dados adicionais são compactados em uma tabela diferente, o que torna difícil para acesso por qualquer meio, exceto por meio da API de provedor de perfil.
- O sistema do provedor permite que você altere o armazenamento de dados de backup, mas o sistema foi projetado para suposições apropriadas para um banco de dados relacional. Você pode escrever um provedor para armazenar informações de associação em um mecanismo de armazenamento não relacionais, como tabelas de armazenamento do Azure, mas, em seguida, você precisa resolver o design relacional escrevendo muito código e muito `System.NotImplementedException` exceções para métodos que não se aplicam a bancos de dados NoSQL.
- Como a funcionalidade de log-in/log-out baseia-se a autenticação de formulários, o sistema de associação não pode usar [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md). OWIN inclui componentes de middleware de autenticação, incluindo suporte para logons usando provedores de identidade externa (como Google Accounts da Microsoft, Facebook, Twitter) e logons usam contas da organização do Active Directory local ou Active Directory do Azure. OWIN também inclui suporte para OAuth 2.0 JWT e CORS.

### <a name="aspnet-simple-membership"></a>Associação simples do ASP.NET

[Associação simples do ASP.NET](../../../web-pages/overview/security/16-adding-security-and-membership.md) foi desenvolvido como um sistema de associação para páginas da Web do ASP.NET. Ele foi lançado com o WebMatrix e Visual Studio 2010 SP1. O objetivo de associação simples foi tornar mais fácil adicionar a funcionalidade de associação a um aplicativo de páginas da Web.

Associação simples facilitam a personalizar as informações de perfil de usuário, mas ainda compartilha os outros problemas com a associação do ASP.NET e ele tem algumas limitações:

- Era difícil de manter dados de sistema de associação em um repositório não relacionais.
- Você não pode usá-lo com OWIN.
- Eles não funcionam bem com provedores de associação ASP.NET existente e não é extensível.

### <a name="aspnet-universal-providers"></a>Provedores Universais ASP.NET

[ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx) foram desenvolvidos para possibilitar manter informações de associação no Microsoft que o banco de dados SQL do Azure e elas também funcionam com o SQL Server Compact. O Universal Providers foi compilado no Entity Framework Code First, que significa que os provedores de Universal pode ser usados para manter dados em qualquer repositório EF com suporte. Com os provedores de Universal, o esquema de banco de dados foi limpo bastante muito bem.

Universal Providers é criado na infraestrutura de associação ASP.NET, portanto eles ainda realizar as mesmas limitações que o provedor de SqlMembership. Ou seja, eles foram projetados para bancos de dados relacionais e é difícil personalizar as informações de perfil e o usuário. Esses provedores ainda usam autenticação de formulários para a funcionalidade de logon e logoff.

## <a name="aspnet-identity"></a>ASP.NET Identity

Como a associação história no ASP.NET evolução ao longo dos anos, a equipe do ASP.NET aprendeu muito com os comentários dos clientes.

A suposição de que os usuários efetuarem logon digitando um nome de usuário e senha que eles foram registrados no seu próprio aplicativo não é mais válida. Web se tornou mais social. Os usuários estão interagindo com o outro em tempo real por meio de canais sociais, como Facebook, Twitter e outros sites sociais. Desenvolvedores desejam que os usuários possam fazer logon com suas identidades sociais para que eles podem ter uma experiência avançada em seus sites. Um sistema de associação modernos deve habilitar o redirecionamento com base em logons para provedores de autenticação, como Facebook, Twitter e outros.

Conforme a evolução de desenvolvimento na web, portanto não os padrões de desenvolvimento na web. Unidade de código do aplicativo de teste se tornou uma preocupação principal para desenvolvedores de aplicativos. 2008 ASP.NET adicionada uma nova estrutura com base no padrão Model-View-Controller (MVC), em parte, para ajudar os desenvolvedores a criar aplicativos de ASP.NET testados de unidade. Os desenvolvedores que desejam unidade teste sua lógica de aplicativo também queira ser capaz de fazer isso com o sistema de associação.

Considerando que essas alterações no desenvolvimento de aplicativos web, ASP.NET Identity foi desenvolvido com os seguintes objetivos:

- **Um sistema de identidade do ASP.NET**

    - Identidade do ASP.NET pode ser usada com todas as estruturas ASP.NET, como ASP.NET MVC, formulários da Web, páginas da Web, API da Web e SignalR.
    - Identidade do ASP.NET pode ser usada durante a criação de aplicativos web, telefone, armazenamento ou híbrida.
- **Facilidade de conexão de dados de perfil sobre o usuário**

    - Você tem controle sobre o esquema de usuário e informações de perfil. Por exemplo, você pode facilmente habilitar o sistema armazenar as datas de nascimento inseridas por usuários quando eles se registram uma conta em seu aplicativo.

- **Controle de persistência**

    - Por padrão, o sistema de identidade do ASP.NET armazena todas as informações de usuário em um banco de dados. Usa o ASP.NET Identity Entity Framework Code First para implementar todo seu mecanismo de persistência.
    - Desde que você controle o esquema de banco de dados, tarefas comuns, como alterar nomes de tabela ou alterar o tipo de dados de chaves primárias é simple.
    - É fácil de plug-in diferentes mecanismos de armazenamento, como o SharePoint, o serviço de tabela de armazenamento do Azure, bancos de dados NoSQL, etc., sem precisar gerar `System.NotImplementedExceptions` exceções.
- **Capacidade de teste de unidade**

    - Identidade do ASP.NET faz com que o aplicativo web mais unidade podem ser testados. Você pode escrever testes de unidade para as partes do seu aplicativo que usam a identidade do ASP.NET.
- **Provedor de função**

    - Não há um provedor de função que permite restringir o acesso a partes do seu aplicativo por funções. Você pode facilmente criar funções como "Admin" e adicionar usuários a funções.
- **Baseada em declarações**

    - Identidade do ASP.NET oferece suporte à autenticação baseada em declarações, onde a identidade do usuário é representada como um conjunto de declarações. Declarações permitem aos desenvolvedores ser muito mais expressivas descrever a identidade do usuário de permitir que funções. Enquanto a associação de função é apenas um booliano (membro ou membro não), uma declaração pode incluir informações detalhadas sobre a identidade do usuário e a associação.
- **Provedores de logon social**

    - Você pode facilmente adicionar logons sociais como Account da Microsoft, Facebook, Twitter, Google e outras pessoas ao seu aplicativo e armazenar os dados específicos do usuário em seu aplicativo.
- **Azure Active Directory**

    - Você pode também adiciona a funcionalidade de log usando o Active Directory do Azure e armazenar os dados específicos do usuário em seu aplicativo. Para obter mais informações, consulte [contas organizacionais](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#orgauth) na criação de projetos Web do ASP.NET no Visual Studio 2013
- **Integração de OWIN**

    - A autenticação do ASP.NET baseia-se agora a owin que pode ser usado em qualquer host do OWIN. Identidade do ASP.NET não tem qualquer dependência em System. Web. Ele é uma estrutura OWIN totalmente compatível e pode ser usado em qualquer aplicativo OWIN hospedado.
    - Identidade do ASP.NET usa autenticação OWIN para log-em/logoff de usuários no site da web. Isso significa que, em vez de usar FormsAuthentication para gerar o cookie, o aplicativo usa CookieAuthentication OWIN para fazer isso.
- **Pacote do NuGet**

    - Identidade do ASP.NET é redistribuída como um pacote NuGet, que é instalado nos modelos ASP.NET MVC, formulários da Web e a API da Web que são fornecidos com o Visual Studio 2013. Você pode baixar este pacote do NuGet da Galeria NuGet.
    - Pacote liberar a identidade do ASP.NET como um NuGet torna mais fácil para a equipe do ASP.NET para iterar sobre novos recursos e correções de bugs e fornecer esses para os desenvolvedores de forma ágil.

## <a name="getting-started-with-aspnet-identity"></a>Introdução ao ASP.NET Identity

Identidade do ASP.NET é usada nos modelos de projeto do Visual Studio 2013 para ASP.NET MVC, formulários da Web, API da Web e SPA. Neste passo a passo, vamos ilustram como os modelos de projeto usam identidade do ASP.NET para adicionar funcionalidade ao se registrar, faça logon e logoff de um usuário.

Identidade do ASP.NET é implementada usando o procedimento a seguir. A finalidade deste artigo é fornecer uma visão geral de alto nível da identidade do ASP.NET; Você pode segui-la passo a passo ou apenas ler os detalhes. Para obter instruções mais detalhadas sobre a criação de aplicativos usando a identidade do ASP.NET, inclusive usando a nova API para adicionar usuários, funções e as informações de perfil, consulte a seção próximas etapas no final deste artigo.

1. Crie um aplicativo ASP.NET MVC com contas individuais. Você pode usar a identidade do ASP.NET no ASP.NET MVC, formulários da Web, API da Web, SignalR etc. Neste artigo vamos começar com um aplicativo ASP.NET MVC.  
  
    ![](introduction-to-aspnet-identity/_static/image1.png)
2. O projeto criado contém os seguintes três pacotes para a identidade do ASP.NET.

    - [`Microsoft.AspNet.Identity.EntityFramework`](http://www.nuget.org/packages/Microsoft.AspNet.Identity.EntityFramework/)  
   Este pacote tem a implementação do Entity Framework da identidade do ASP.NET que persistirá os dados de identidade do ASP.NET e o esquema para o SQL Server.
    - [`Microsoft.AspNet.Identity.Core`](http://www.nuget.org/packages/Microsoft.AspNet.Identity.Core/)  
   Este pacote tem interfaces principais para a identidade do ASP.NET. Esse pacote pode ser usado para gravar uma implementação para a identidade do ASP.NET que persistência diferentes destinos armazena como armazenamento de tabela do Azure, NoSQL bancos de dados etc.
    - [`Microsoft.AspNet.Identity.OWIN`](http://www.nuget.org/packages/Microsoft.AspNet.Identity.Owin/)  
   Este pacote contém a funcionalidade que é usada para o plug-in de autenticação OWIN com a identidade do ASP.NET em aplicativos ASP.NET. Isso é usado quando você adicionar log na funcionalidade ao seu aplicativo e as chamadas no middleware de autenticação de Cookie OWIN para gerar um cookie.
3. Criar um usuário.  
   Iniciar o aplicativo e, em seguida, clique no **registrar** link para criar um usuário. A imagem a seguir mostra a página de registro que coleta o nome de usuário e senha.  
  
    ![](introduction-to-aspnet-identity/_static/image2.png)  
  
   Quando o usuário clica o **registrar** botão, o `Register` ação do controlador de conta cria o usuário chamando a API de identidade do ASP.NET, como destacado abaixo:

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample1.cs?highlight=8-9)]
4. Iniciar sessão.  
   Se o usuário foi criado com êxito, ela é registrada pelo `SignInAsync` método.  

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample2.cs?highlight=12)]

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample3.cs?highlight=5-6)]

   O código realçado acima no `SignInAsync` método gera uma [ClaimsIdentity](https://msdn.microsoft.com/library/system.security.claims.claimsidentity.aspx). Como identidade do ASP.NET e autenticação de Cookie OWIN são sistema baseado em declarações, o framework requer o aplicativo para gerar uma ClaimsIdentity para o usuário. ClaimsIdentity tem informações sobre todas as declarações para o usuário, como as funções que o usuário pertence. Você também pode adicionar mais declarações para o usuário neste estágio.  
  
   O código realçado abaixo no `SignInAsync` método assina o usuário usando o AuthenticationManager do OWIN e chamar `SignIn` e passando o ClaimsIdentity.  

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample4.cs?highlight=8-11)]
5. Fazer logoff.  
   Clique o **logoff** link chama a ação de LogOff no controlador de conta. 

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample5.cs?highlight=6)]

   Código realçada acima mostra o OWIN `AuthenticationManager.SignOut` método. Isso equivale a [FormsAuthentication.SignOut](https://msdn.microsoft.com/library/system.web.security.formsauthentication.signout.aspx) método usado pelo [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) módulo em formulários da Web.

## <a name="components-of-aspnet-identity"></a>Componentes da identidade do ASP.NET

O diagrama a seguir mostra os componentes do sistema de identidade do ASP.NET (clique no [isso](introduction-to-aspnet-identity/_static/image3.png) ou no diagrama para aumentá-la). Os pacotes em verde compõem o sistema de identidade do ASP.NET. Todos os outros pacotes são dependências que são necessárias para usar o sistema de identidade do ASP.NET em aplicativos ASP.NET.

[![](introduction-to-aspnet-identity/_static/image5.png)](introduction-to-aspnet-identity/_static/image4.png)

A seguir está uma breve descrição dos pacotes do NuGet não mencionadas anteriormente:

- [Microsoft.Owin.Security.Cookies](http://www.nuget.org/packages/Microsoft.Owin.Security.Cookies/)  
 Middleware que permite que um aplicativo para usar o cookie de autenticação, semelhante ao ASP com base em. Autenticação de formulários do NET.
- [EntityFramework](http://www.nuget.org/packages/EntityFramework/)  
 Entity Framework é uma tecnologia de acesso a dados recomendada da Microsoft para bancos de dados relacionais.

## <a name="migrating-from-membership-to-aspnet-identity"></a>Migrando da associação para a identidade do ASP.NET

Esperamos que em breve fornecem orientação sobre como migrar seus aplicativos existentes que usam associação do ASP.NET ou associação simples para o novo sistema de identidade do ASP.NET.

## <a name="next-steps"></a>Próximas etapas

- [Criar um aplicativo ASP.NET MVC 5 com o Facebook e Google OAuth2 e logon do OpenID](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)  
 O tutorial usa a API de identidade do ASP.NET para adicionar informações de perfil para o banco de dados de usuário e como autenticar com o Google e Facebook.
- [Criar um aplicativo ASP.NET MVC com autenticação e o banco de dados SQL e implantar o serviço de aplicativo do Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)  
 Este tutorial mostra como usar a API de identidade para adicionar usuários e funções.
- [Contas de usuário individuais](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#indauth) na criação de projetos Web ASP.NET no Visual Studio 2013
- [Contas organizacionais](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#orgauth) na criação de projetos Web ASP.NET no Visual Studio 2013
- [Personalizando informações de perfil no ASP.NET Identity em modelos do VS 2013](https://blogs.msdn.com/b/webdev/archive/2013/10/16/customizing-profile-information-in-asp-net-identity-in-vs-2013-templates.aspx)
- [Obter mais informações dos provedores sociais usados nos modelos de projeto do VS 2013](https://blogs.msdn.com/b/webdev/archive/2013/10/16/get-more-information-from-social-providers-used-in-the-vs-2013-project-templates.aspx)
- [https://github.com/rustd/AspnetIdentitySample](https://github.com/rustd/AspnetIdentitySample)  
 Aplicativo de exemplo que mostra como adicionar funções básicas e suporte ao usuário e como fazer o gerenciamento de usuário e funções.
