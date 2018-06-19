---
uid: web-forms/overview/older-versions-security/introduction/security-basics-and-asp-net-support-cs
title: Noções básicas sobre segurança e suporte do ASP.NET (c#) | Microsoft Docs
author: rick-anderson
description: Este é o primeiro tutorial em uma série de tutoriais que irá explorar técnicas para autenticar os visitantes por meio de um formulário da web, autorizando o acesso a determinado...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/13/2008
ms.topic: article
ms.assetid: 07e15538-2f29-40c6-b2e7-e6115075ac83
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/security-basics-and-asp-net-support-cs
msc.type: authoredcontent
ms.openlocfilehash: 2b89a8b0dd88505c1d63054db508590c26684158
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30889785"
---
<a name="security-basics-and-aspnet-support-c"></a>Noções básicas sobre segurança e suporte do ASP.NET (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar PDF](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial01_Basics_cs.pdf)

> Este é o primeiro tutorial em uma série de tutoriais que irá explorar técnicas para autenticar os visitantes por meio de um formulário da web, autorizando o acesso a páginas específicas e funcionalidade e gerenciar contas de usuário em um aplicativo ASP.NET.


## <a name="introduction"></a>Introdução

O que é o algo fóruns, sites de comércio eletrônico, sites de email online, sites do portal e sites de rede social que todos têm em comum? Todos eles oferecem *contas de usuário*. Sites que oferecem as contas de usuário devem fornecer um número de serviços. No mínimo, novos visitantes precisam ser capaz de criar uma conta e visitantes de retorno devem ser capazes de fazer logon. Esses aplicativos web podem tomar decisões com base no usuário conectado: algumas páginas ou ações podem ser restritas a somente conectados em usuários, ou para um certo subconjunto de usuários. outras páginas podem mostrar informações específicas para o usuário conectado, ou podem mostrar mais ou menos informações, dependendo do que o usuário está exibindo a página.

Este é o primeiro tutorial em uma série de tutoriais que irá explorar técnicas para autenticar os visitantes por meio de um formulário da web, autorizando o acesso a páginas específicas e funcionalidade e gerenciar contas de usuário em um aplicativo ASP.NET. Ao longo destes tutoriais, examinaremos como:

- Identificar e registrar usuários para um site
- Use o ASP. Estrutura de associação da rede para gerenciar contas de usuário
- Criar, atualizar e excluir contas de usuário
- Limitar o acesso a uma página da web, o diretório ou a funcionalidade específica com base no usuário conectado
- Use o ASP. Framework de funções do .NET para associar contas de usuário a funções
- Gerenciar funções de usuário
- Limitar o acesso a uma página da web, o diretório ou a funcionalidade específica com base na função do usuário conectado
- Personalizar e estender o ASP. Os controles de segurança de rede Web

Esses tutoriais são orientados para ser conciso e fornecem instruções passo a passo com capturas de tela suficiente para orientá-lo pelo processo visualmente. Cada tutorial está disponível em c# e Visual Basic versões e inclui um download do código completo usado. (Este tutorial primeiro enfoca os conceitos de segurança de um ponto de vista de alto nível e, portanto, não contém qualquer código associado.)

Neste tutorial, discutiremos os conceitos de segurança importantes e quais recursos estão disponíveis no ASP.NET para auxiliar na implementação de formulários de autenticação, autorização, contas de usuário e funções. Vamos começar!

> [!NOTE]
> A segurança é um aspecto importante de qualquer aplicativo que abrange físicos, tecnológicas e política decisões e requer um alto grau de planejamento e conhecimento do domínio. Esta série de tutoriais não destina-se como um guia para o desenvolvimento de aplicativos da web seguros. Em vez disso, ele se concentra especificamente em formulários de autenticação, autorização, contas de usuário e funções. Enquanto alguns conceitos de segurança que são exibidas ao redor esses problemas são discutidos nesta série, outros são deixados inexplorado.


## <a name="authentication-authorization-user-accounts-and-roles"></a>Autenticação, autorização, contas de usuário e funções

Autenticação, autorização, contas de usuário e funções são as quatro condições que serão usadas com muita frequência, em toda a série de tutoriais, para que eu gostaria de fazer um pouco para definir estes termos de dentro do contexto de segurança da web. Em um modelo cliente-servidor, como a Internet, há muitos cenários em que o servidor deve identificar o cliente faz a solicitação. *Autenticação* é o processo de verificar a identidade do cliente. Um cliente que foi identificado com êxito será considerado *autenticado*. Um cliente será considerado *não autenticado* ou *anônimo*.

Sistemas de autenticação segura envolvem pelo menos um dos três observações a seguir: [algo que você sabe, algo que você possui ou algo que você está](http://www.cs.cornell.edu/Courses/cs513/2005fa/NNLauthPeople.html). A maioria dos aplicativos da web dependem de algo que o cliente sabe, como uma senha ou um PIN. As informações usadas para identificar um usuário - seu nome de usuário e senha, por exemplo - são chamados de *credenciais*. Esta série de tutoriais enfoca *autenticação de formulários*, que é um modelo de autenticação, onde os usuários fazer logon site ao fornecer suas credenciais em um formulário de página da web. Todos os experimentamos esse tipo de autenticação antes. Vá para qualquer site de comércio eletrônico. Quando você estiver pronto para check-out, você será solicitado a entrar digitando seu nome de usuário e senha nas caixas de texto em uma página da web.

Além de identificar os clientes, um servidor talvez seja necessário limitar quais recursos ou funcionalidades estão acessíveis dependendo do cliente faz a solicitação. *Autorização* é o processo de determinar se um determinado usuário tem autoridade para acessar um recurso específico ou funcionalidade.

Um *conta de usuário* é um repositório para manter informações sobre um determinado usuário. Contas de usuário com o mínimo devem incluir informações que identificam exclusivamente o usuário, como nome de logon do usuário e senha. Com essas informações essenciais, contas de usuário podem incluir itens como: endereço de email do usuário; a data e hora que a conta foi criada; a data e hora, última registradas no; nome e sobrenome; número de telefone; e o endereço para correspondência. Ao usar autenticação de formulários, informações de conta de usuário normalmente são armazenadas em um banco de dados relacional, como o Microsoft SQL Server.

Aplicativos da Web que dá suporte a contas de usuário podem, opcionalmente, agrupar usuários em *funções*. Uma função é simplesmente um rótulo aplicado a um usuário e fornece uma abstração para definir regras de autorização e a funcionalidade de nível de página. Por exemplo, um site pode incluir uma função de administrador com as regras de autorização que impedem que somente um administrador acesse um determinado conjunto de páginas da web. Além disso, uma variedade de páginas que são acessíveis a todos os usuários (incluindo a não administradores) pode exibir dados adicionais ou oferecer funcionalidade adicional quando visitados pelos usuários na função de administradores. Uso de funções, podemos definir essas regras de autorização em uma função por função, em vez de por usuário.

## <a name="authenticating-users-in-an-aspnet-application"></a>Autenticação de usuários em um aplicativo ASP.NET

Quando um usuário digita uma URL em seu navegador janela endereço ou clica em um link, o navegador faz uma [protocolo HTTP (Hypertext Transfer)](http://en.wikipedia.org/wiki/HTTP) a solicitação para o servidor web para o conteúdo especificado, seja ele um ASP.NET página, uma imagem, um JavaScript arquivo ou qualquer outro tipo de conteúdo. O servidor web é designado para retornar o conteúdo solicitado. Dessa forma, ele deve determinar várias coisas sobre a solicitação, incluindo quem fez a solicitação e se a identidade está autorizada para recuperar o conteúdo solicitado.

Por padrão, os navegadores enviam solicitações HTTP que não têm qualquer tipo de informações de identificação. Mas, se o navegador incluir informações de autenticação do servidor web inicia o fluxo de trabalho de autenticação, que tenta identificar o cliente faz a solicitação. As etapas do fluxo de trabalho de autenticação dependem do tipo de autenticação usado pelo aplicativo da web. ASP.NET oferece suporte a três tipos de autenticação: Windows, Passport e formulários. Esta série de tutorial concentra-se na autenticação de formulários, mas vamos comparar e contrastar repositórios de usuário de autenticação do Windows e de fluxo de trabalho.

### <a name="authentication-via-windows-authentication"></a>Autenticação por meio da autenticação do Windows

O fluxo de trabalho de autenticação do Windows usa uma das seguintes técnicas de autenticação:

- Autenticação básica
- Autenticação Digest
- Autenticação integrada do Windows

Todas as três técnicas funcionam quase da mesma maneira: quando um não autorizado, chega de solicitação anônima, o servidor da web envia de volta uma resposta HTTP que indica que a autorização é necessária para continuar. O navegador exibe uma caixa de diálogo modal que solicita ao usuário seu nome de usuário e senha (consulte a Figura 1). Essa informação é enviada para o servidor web por meio de um cabeçalho HTTP.


![Caixa de diálogo Modal solicita ao usuário credenciais](security-basics-and-asp-net-support-cs/_static/image1.png)

**Figura 1**: caixa de diálogo Modal solicita ao usuário credenciais


As credenciais fornecidas são validadas em relação o repositório de usuário do Windows do servidor web. Isso significa que cada usuário autenticado no seu aplicativo web deve ter uma conta do Windows em sua organização. Isso é comuns em cenários de intranet. Na verdade, ao usar a autenticação integrada do Windows em uma configuração de intranet, o navegador fornece automaticamente o servidor web com as credenciais usadas para fazer logon na rede, suprimindo, assim, a caixa de diálogo mostrada na Figura 1. Enquanto a autenticação do Windows é ideal para aplicativos de intranet, é geralmente impraticável para aplicativos da Internet porque você não deseja criar contas do Windows para cada usuário que se inscreve no seu site.

### <a name="authentication-via-forms-authentication"></a>Autenticação por meio de autenticação de formulários

Autenticação de formulários, por outro lado, é ideal para aplicativos web da Internet. Lembre-se que a autenticação de formulários identifica o usuário, solicitando que eles digitem suas credenciais por meio de um formulário da web. Consequentemente, quando um usuário tenta acessar um recurso não autorizado, eles são redirecionados automaticamente para a página de logon em que eles podem digitar suas credenciais. As credenciais enviadas, em seguida, são validadas em relação a um repositório de usuário personalizada - normalmente um banco de dados.

Depois de verificar as credenciais enviadas, um *tíquete de autenticação de formulários* é criado para o usuário. Esse tíquete indica que o usuário foi autenticado e inclui informações de identificação, como o nome de usuário. O tíquete de autenticação de formulários (geralmente) é armazenado como um cookie no computador cliente. Portanto, subsequentes visitas ao site incluem o tíquete de autenticação de formulários na solicitação HTTP, habilitando o aplicativo web identificar o usuário depois de ter conectado.

A Figura 2 ilustra o fluxo de trabalho de autenticação de formulários de um ponto de vantagem de alto nível. Observe como as partes de autenticação e autorização no ASP.NET atuam como duas entidades separadas. O sistema de autenticação de formulários identifica o usuário (ou relatórios que são anônimas). O sistema de autorização é o que determina se o usuário tem acesso ao recurso solicitado. Se o usuário está autorizado (como estão na Figura 2 ao tentar anonimamente visite ProtectedPage.aspx), o sistema de autorização relata que o usuário foi negado, fazendo com que os formulários de sistema de autenticação redirecionar automaticamente o usuário para a página de logon.

Quando o usuário tiver conectado com êxito, as solicitações HTTP subsequentes incluem o tíquete de autenticação de formulários. O sistema de autenticação de formulários simplesmente identifica o usuário - é o sistema de autorização que determina se o usuário pode acessar o recurso solicitado.


![O fluxo de trabalho de autenticação de formulários](security-basics-and-asp-net-support-cs/_static/image2.png)

**Figura 2**: O fluxo de trabalho de autenticação de formulários


Podemos aprofundarão sobre a autenticação de formulários muito mais detalhadamente nos próximos dois tutoriais,[uma visão geral de formulários de autenticação](an-overview-of-forms-authentication-cs.md) e [configuração de autenticação de formulários e tópicos avançados](forms-authentication-configuration-and-advanced-topics-cs.md). Para obter mais informações sobre o ASP. Opções de autenticação do NET, consulte [autenticação ASP.NET](https://msdn.microsoft.com/library/eeyk640h.aspx).

## <a name="limiting-access-to-web-pages-directories-and-page-functionality"></a>Limitando o acesso a páginas da Web, diretórios e funcionalidade de página

O ASP.NET inclui duas maneiras de determinar se um determinado usuário tem autoridade para acessar um determinado arquivo ou diretório:

- **Autorização de arquivo** - como páginas ASP.NET e serviços da web são implementados como arquivos que residem no sistema de arquivos do servidor web, acesso a esses arquivos podem ser especificados por meio de listas de controle de acesso (ACLs). Autorização do arquivo é mais comumente usada com autenticação do Windows porque as ACLs são permissões que se aplicam a contas do Windows. Ao usar a autenticação de formulários, todas as solicitações de nível de sistema de arquivo e sistema operacional são executadas pela mesma conta do Windows, independentemente do usuário visitar o site.
- **Autorização de URL**-com autorização de URL, o desenvolvedor de página especifica regras de autorização no Web. config. Essas regras de autorização especificam quais usuários ou funções têm permissão para acessar ou recebem acesso a certas páginas ou diretórios no aplicativo.

Autorização de arquivo e a autorização de URL definem regras de autorização para acessar uma página específica do ASP.NET ou para todas as páginas ASP.NET em um diretório específico. Usando essas técnicas, pode instruir o ASP.NET para negar as solicitações para uma página específica para um usuário específico, ou permitir o acesso a um conjunto de usuários e negar acesso a todos os outros. E cenários em que todos os usuários podem acessar a página, mas a funcionalidade da página depende do usuário? Por exemplo, muitos sites que dão suporte a contas de usuário têm páginas que exibem conteúdo diferente ou dados para usuários autenticados em relação a usuários anônimos. Um usuário anônimo pode ver um link para fazer logon no site, ao passo que um usuário autenticado em vez disso, verá uma mensagem como, bem-vindo de volta, *Username* juntamente com um link para fazer logoff. Outro exemplo: ao exibir um item em um site de leilões você veja informações diferentes dependendo se você for um ofertante ou um auctioning o item.

Esses ajustes de nível de página podem ser feitos declarativamente ou programaticamente. Para mostrar o conteúdo diferente para anônimo que os usuários autenticados, basta arrastar um [controle LoginView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginview.aspx) para a página e digite o conteúdo apropriado para seus modelos LoginView e LoggedInTemplate. Como alternativa, você pode programaticamente determinar se a solicitação atual é autenticada, quem é o usuário e as funções que eles pertencem (se houver). Você pode usar essas informações para, em seguida, mostrar ou ocultar colunas em uma grade ou painéis na página.

Esta série inclui três tutoriais que se concentrar na autorização. ***Autorização baseada em usuário***examina como limitar o acesso a uma página ou páginas em um diretório para contas de usuário específico. ***Autorização com base no função*** examina fornecendo as regras de autorização na função de nível; por fim, o ***exibindo conteúdo com base no momento conectado no usuário*** tutorial explora modificando um determinado conteúdo e funcionalidade com base no usuário visitar a página da página. Para obter mais informações sobre o ASP. Opções de autorização do NET, consulte [autorização ASP.NET](https://msdn.microsoft.com/library/wce3kxhd.aspx).


## <a name="user-accounts-and-roles"></a>Contas de usuário e funções

AS PÁGINAS ASP. Autenticação de formulários do NET fornece uma infraestrutura para os usuários fazer logon um site e têm seu estado autenticado memorizado entre visitas à página. E a autorização de URL oferece uma estrutura para limitar o acesso a arquivos específicos ou pastas em um aplicativo ASP.NET. Nenhum recurso, no entanto, fornece um meio para armazenar informações de conta de usuário ou gerenciar funções.

Antes do ASP.NET 2.0, os desenvolvedores foram responsáveis por criar seus próprios armazenamentos de usuário e a função. Também estivesse em gancho para criação de interfaces do usuário e escrever o código de usuário essencial páginas relacionadas à conta como a página de logon e a página para criar uma nova conta, entre outros. Sem qualquer estrutura de conta de usuário internas no ASP.NET, cada desenvolvedor Implementando contas de usuários tinham para chegar ao seus próprio decisões de design em perguntas, como armazenar senhas ou outras informações confidenciais? e quais as diretrizes impõem relativas a força e o comprimento da senha?

Hoje, a implementação de contas de usuário em um aplicativo ASP.NET é obrigado muito mais simples para o *framework associação* e internos controles da Web de logon. A estrutura de associação é uma série de classes de [namespace System.Web.Security](https://msdn.microsoft.com/library/system.web.security.aspx) que fornecem funcionalidade para executar tarefas relacionadas à conta de usuário essenciais. A classe de chave na estrutura de associação é o [classe associação](https://msdn.microsoft.com/library/system.web.security.membership.aspx), que tem métodos, como:

- CreateUser
- DeleteUser
- GetAllUsers
- GetUser
- UpdateUser
- ValidateUser

A estrutura de associação usa o [modelo de provedor](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), que separa claramente a API da estrutura de associação da sua implementação. Isso permite aos desenvolvedores usar uma API, mas permite que eles usam uma implementação que atenda às necessidades de seu aplicativo personalizado. Em resumo, a classe de associação define a funcionalidade essencial do framework (métodos, propriedades e eventos), mas, na verdade, não forneça quaisquer detalhes de implementação. Em vez disso, os métodos da classe Membership chamar o provedor configurado, que é o que executa o trabalho real. Por exemplo, quando CreateUser método a classe de associação do é chamada, a classe de associação não sabe os detalhes do repositório do usuário. Ele não sabe se os usuários estão sendo mantidos em um banco de dados ou em um arquivo XML ou em algum outro repositório. A classe de associação examina a configuração do aplicativo da web para determinar qual provedor para delegar a chamada para, e essa classe de provedor é responsável pela criação, na verdade, a nova conta de usuário no repositório do usuário apropriada. Essa interação é ilustrada na Figura 3.

Microsoft fornece duas classes de provedor de associação no .NET Framework:

- [ActiveDirectoryMembershipProvider](https://msdn.microsoft.com/library/system.web.security.activedirectorymembershipprovider.aspx) -implementa a API de associação em servidores do Active Directory e o aplicativo de modo ADAM (Active Directory).
- [SqlMembershipProvider](https://msdn.microsoft.com/library/system.web.security.sqlmembershipprovider.aspx) -implementa a API de associação em um banco de dados do SQL Server.

Esta série de tutoriais concentra-se exclusivamente no SqlMembershipProvider.


[![O modelo permite que diferentes implementações do provedor para ser perfeitamente conectado na estrutura do&lt;/ forte&gt;](security-basics-and-asp-net-support-cs/_static/image4.png)](security-basics-and-asp-net-support-cs/_static/image3.png)

**Figura 03**: O modelo permite que diferentes implementações do provedor ser perfeitamente conectado para o Framework ([clique para exibir a imagem em tamanho normal](security-basics-and-asp-net-support-cs/_static/image5.png))


O benefício do modelo do provedor é implementações alternativas podem ser desenvolvidas pela Microsoft, fornecedores de terceiros ou desenvolvedores individuais e conectadas diretamente a estrutura de associação. Por exemplo, a Microsoft lançou [um provedor de associação para bancos de dados do Microsoft Access](https://download.microsoft.com/download/5/5/b/55bc291f-4316-4fd7-9269-dbf9edbaada8/sampleaccessproviders.vsi). Para obter mais informações sobre os provedores de associação, consulte o [Kit de ferramentas do provedor](https://msdn.microsoft.com/asp.net/aa336558.aspx), que inclui um passo a passo de provedores de associação, provedores personalizados de exemplo, mais de 100 páginas de documentação sobre o modelo de provedor e o concluir o código-fonte para os provedores de associação internos (ou seja, ActiveDirectoryMembershipProvider e SqlMembershipProvider).

O ASP.NET 2.0 também introduziu o framework de funções. Como o framework de associação, estrutura de funções é criada sobre o modelo de provedor. Sua API é exposta por meio de [classe funções](https://msdn.microsoft.com/library/system.web.security.roles.aspx) e o .NET Framework vem com três classes de provedor:

- [AuthorizationStoreRoleProvider](https://msdn.microsoft.com/library/system.web.security.authorizationstoreroleprovider.aspx) -gerencia informações de função em um repositório de política do Gerenciador de autorização, como o Active Directory ou ADAM.
- [SqlRoleProvider](https://msdn.microsoft.com/library/system.web.security.sqlroleprovider.aspx) -implementa funções em um banco de dados do SQL Server.
- [WindowsTokenRoleProvider](https://msdn.microsoft.com/library/system.web.security.windowstokenroleprovider.aspx) -associa informações de função com base no grupo do Windows do visitante. Normalmente, esse método é usado com a autenticação do Windows.

Esta série de tutorial concentra-se exclusivamente no SqlRoleProvider.

Como o modelo de provedor inclui uma única API voltada para frente (as classes de associação e funções), é possível criar a funcionalidade em torno dessa API sem precisar se preocupar sobre os detalhes de implementação - as são tratadas pelos provedores selecionados por página desenvolvedor. Permite que essa API unificada para a Microsoft e outros fornecedores criar controles da Web que fazem interface com as estruturas de associação e funções. O ASP.NET é fornecido com um número de [controles da Web de logon](https://msdn.microsoft.com/library/ms178329.aspx) para implementar as interfaces de usuário de conta de usuário comuns. Por exemplo, o [controle de logon](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.aspx) solicita ao usuário as credenciais, valida e, em seguida, registra-los no através da autenticação de formulários. O [controle LoginView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginview.aspx) oferece modelos de exibição de marcação diferente aos usuários anônimos versus usuários autenticados ou marcação diferente com base na função do usuário. E o [controle CreateUserWizard](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.aspx) fornece uma interface de usuário passo a passo para criar uma nova conta de usuário.

Nos bastidores vários controles de logon interagem com as estruturas de associação e funções. A maioria dos controles de logon pode ser implementada sem a necessidade de escrever uma única linha de código. Vamos examinar esses controles mais detalhadamente em tutoriais futuros, incluindo técnicas para estender e personalizar sua funcionalidade.

## <a name="summary"></a>Resumo

Todos os aplicativos web que oferecem suporte a contas de usuário exigem recursos semelhantes, como: a capacidade dos usuários entrar e ter seu log de status memorizados entre visitas de página; uma página da web para os visitantes novo criar uma conta; e a capacidade para o desenvolvedor de página para especificar quais recursos, dados e funcionalidades estão disponíveis para quais usuários ou funções. As tarefas de autenticação e autorização de usuários e de gerenciamento de funções e contas de usuário é extremamente fácil de realizar em aplicativos ASP.NET graças a autenticação de formulários, autorização de URL e as estruturas de associação e funções.

Ao longo dos vários tutoriais do próximos vamos examinar esses aspectos ao criar um aplicativo da web de trabalho de ponta a ponta em um passo a passo. O tutorial de duas, exploraremos autenticação de formulários em detalhes. Veremos o fluxo de trabalho de autenticação de formulários em ação, examiná o tíquete de autenticação de formulários, discuta questões de segurança e saber como configurar o sistema de autenticação de formulários - todos durante a criação de um aplicativo web que permite que os visitantes fazer logon e logoff.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [O ASP.NET 2.0 associação, funções, autenticação de formulários e recursos de segurança](https://weblogs.asp.net/scottgu/ASP.NET-2.0-Membership_2C00_-Roles_2C00_-Forms-Authentication_2C00_-and-Security-Resources-)
- [Diretrizes de segurança 2.0 do ASP.NET](https://msdn.microsoft.com/library/ms998258.aspx)
- [Autenticação do ASP.NET](https://msdn.microsoft.com/library/eeyk640h.aspx)
- [Autorização de ASP.NET](https://msdn.microsoft.com/library/wce3kxhd.aspx)
- [Visão geral de controles de logon do ASP.NET](https://msdn.microsoft.com/library/ms178329.aspx)
- [Examinando o ASP.NET de 2.0 associação, funções e perfil](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Como usar protege Meu Site usando associações e funções?](https://asp.net/learn/videos/video-45.aspx) (Vídeo)
- [Introdução a associação](https://msdn.microsoft.com/library/yh26yfzy.aspx)
- [MSDN Security Developer Center](https://msdn.microsoft.com/security/default.aspx)
- [Professional ASP.NET 2.0 segurança, associação e gerenciamento de função](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html) (ISBN: 978-0-7645-9698-8)
- [Kit de ferramentas do provedor](https://msdn.microsoft.com/asp.net/aa336558.aspx)

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla. com](http://www.4guysfromrolla.com), trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado em [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisado por vários revisores úteis. Revisor levar para este tutorial foi este tutorial série foi revisada por vários revisores úteis. Revisores levar para este tutorial incluem Alicja Maziarz, John Suru e Teresa Murphy. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha no [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Avançar](an-overview-of-forms-authentication-cs.md)
