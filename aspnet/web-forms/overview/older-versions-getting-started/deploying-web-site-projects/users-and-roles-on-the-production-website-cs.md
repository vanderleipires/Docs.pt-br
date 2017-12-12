---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-cs
title: "Usuários e funções no site de produção (c#) | Microsoft Docs"
author: rick-anderson
description: "A ferramenta de administração de site do ASP.NET (WSAT) fornece uma interface de usuário baseada na web para definir as configurações de associação e funções e para a criação, edição, um..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: dbc54313-5d05-4285-98b3-726edea6d0c9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-cs
msc.type: authoredcontent
ms.openlocfilehash: 0825b6bd6ca8d75f90cb7c4079e3af0213c5c4e9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="users-and-roles-on-the-production-website-c"></a>Usuários e funções no site de produção (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial16_CustomAWAT_cs.pdf)

> A ferramenta de administração de site do ASP.NET (WSAT) fornece uma interface de usuário baseada na web para definir as configurações de associação e funções e para criar, editar e excluir usuários e funções. Infelizmente, o WSAT só funciona quando visitado do localhost, que significa que você não conseguir acessar a ferramenta de administração do site de produção por meio de seu navegador. A boa notícia é que há soluções alternativas que tornam possível gerenciar usuários e funções na produção. Este tutorial examina essas soluções alternativas e outros.


## <a name="introduction"></a>Introdução

O ASP.NET 2.0 introduziu um número de *serviços de aplicativo*, que são um conjunto de serviços de bloco de construção que você pode adicionar ao seu aplicativo web. Adicionamos os serviços de associação e funções para o site do catálogo revisões de volta a [ *Configurando um site que usa serviços de aplicativos* tutorial](configuring-a-website-that-uses-application-services-cs.md). O serviço de associação facilita criando e gerenciando contas de usuário. o serviço de funções oferece uma API para categorizar os usuários em grupos. O site de revisões de livros tem três contas de usuário - Scott, Jisun e Alice - e uma única função de administrador, com Scott e Jisun na função de administrador.

AS PÁGINAS ASP. Serviços de aplicativo do NET não são associados a uma implementação específica. Em vez disso, você pode instruir os serviços do aplicativo para usar um determinado *provedor*, e esse provedor implementa o serviço usando uma tecnologia específica. Configuramos o aplicativo web de revisões de livros para usar o `SqlMembershipProvider` e `SqlRoleProvider` provedores para os serviços de associação e funções. Esses dois provedores de armazenam informações de conta e a função de usuário em um banco de dados do SQL Server e são os provedores mais comumente usados para aplicativos baseados na Internet web hospedados em uma empresa de hospedagem na web.

Um desafio comum para desenvolvedores que usam os serviços de associação e funções está gerenciando os usuários e funções no ambiente de produção. Como excluir uma conta de usuário do site de produção, adicionar uma nova função ou adicionar um usuário existente a uma função existente? Este tutorial explora técnicas diferentes para gerenciar usuários e funções no site de produção.

## <a name="using-the-aspnet-web-site-administration-tool"></a>Usando a ferramenta de administração de Site da Web do ASP.NET

O ASP.NET inclui um [ferramenta de administração de Site](https://msdn.microsoft.com/en-us/library/yy40ytx0.aspx) (WSAT) que torna mais fácil de criar e gerenciar contas de usuário e funções e especificar regras de autorização baseada em função e usuário. Para usar o WSAT, clique no ícone de configuração do ASP.NET no Gerenciador de soluções, ou vá para o menu projeto ou site e escolha a opção de configuração do ASP.NET. Qualquer abordagem inicia um navegador da web e aponta para o WSAT em um endereço como:`http://localhost:portNumber/asp.netwebadminfiles/default.aspx?applicationPhysicalPath=pathToApplication`

O WSAT é dividido em três seções:

- **Segurança** -gerenciar usuários, funções e regras de autorização.
- **Aplicaçãoção da** -gerenciar o &lt;appSettings&gt; e configurações de SMTP aqui. Você pode também colocar o aplicativo offline e gerenciar as configurações aqui de rastreamento e depuração, bem como especificar a página de erro personalizada padrão.
- **ProviderConfiguration** -configure os provedores usados pelos serviços de aplicativo.

A seção de segurança (mostrado em **Figura 1**) inclui links para criar novos usuários, gerenciamento de usuários, criar e gerenciar funções e criar e gerenciar regras de acesso. Aqui você pode adicionar uma nova função no sistema, excluir um usuário existente, ou de adicionar ou remover funções de uma conta de usuário específico.

[![](users-and-roles-on-the-production-website-cs/_static/image2.png)](users-and-roles-on-the-production-website-cs/_static/image1.png)

**Figura 1**: A seção de segurança WSAT inclui opções para gerenciar usuários e funções  
([Clique para exibir a imagem em tamanho normal](users-and-roles-on-the-production-website-cs/_static/image3.png))

Infelizmente, o WSAT é acessível apenas localmente. Não é possível consultar o WSAT no seu site de produção remoto; Se você visitar `www.yoursite.com/asp.netwebadminfiles/default.aspx` obter uma resposta de 404 não encontrado. O código que alimenta o WSAT usa o `Membership` e `Roles` classes do .NET Framework para criar, editar e excluir usuários e funções. Essas classes Consulte informações de configuração do aplicativo da web para determinar qual provedor será usado; volta o [ *Configurando um site que usa serviços de aplicativos* tutorial](configuring-a-website-that-uses-application-services-cs.md) nós configuramos o site de revisões de livros para usar o `SqlMembershipProvider` e `SqlRoleProvider` provedores. Isso implicava adicionando `<membership>` e `<roleManager>` seções para `Web.config`.

[!code-xml[Main](users-and-roles-on-the-production-website-cs/samples/sample1.xml)]

Observe que o `<membership>` e `<roleManager>` seções de referência a `SqlMembershipProvider` e `SqlRoleProvider` provedores em sua `type` atributo, respectivamente. Esses provedores de armazenam as informações de função e o usuário em um banco de dados do SQL Server especificado. O banco de dados usado por esses provedores é especificado pelo `connectionStringName` atributo, `ReviewsConnectionString`, que é definido no `~/ConfigSections/databaseConnectionStrings.config` arquivo. Lembre-se de que o `databaseConnectionStrings.config` arquivo no ambiente de desenvolvimento contém a cadeia de caracteres de conexão para o banco de dados de desenvolvimento, enquanto o `databaseConnectionStrings.config` arquivo em produção contém a cadeia de caracteres de conexão para o banco de dados de produção.

Em resumo, o WSAT deve ser acessado localmente por meio do ambiente de desenvolvimento, e funciona com as informações de usuário e a função no banco de dados especificado no `databaseConnectionStrings.config` arquivo. Consequentemente, se alterarmos as informações de cadeia de caracteres de conexão no `databaseConnectionStrings.config` arquivo no ambiente de desenvolvimento podemos usar o WSAT localmente para gerenciar usuários e funções no ambiente de produção.

Para ilustrar essa funcionalidade, abra o `databaseConnectionStrings.config` no Visual Studio no ambiente de desenvolvimento e substitua a cadeia de caracteres de conexão de banco de dados de desenvolvimento com a cadeia de caracteres de conexão de banco de dados de produção. Em seguida, inicie o WSAT, vá na guia Segurança e adicionar um novo usuário chamado Sam com senha "password"! (menor as marcas de aspas). **Figura 2** mostra a tela WSAT ao criar essa conta.

[![](users-and-roles-on-the-production-website-cs/_static/image5.png)](users-and-roles-on-the-production-website-cs/_static/image4.png)

**Figura 2**: criar um novo usuário chamado Sam no ambiente de produção  
([Clique para exibir a imagem em tamanho normal](users-and-roles-on-the-production-website-cs/_static/image6.png))

Como nós alteramos a cadeia de caracteres de conexão no `databaseConnectionStrings.config` para apontar para o servidor de banco de dados de produção, Sam foi adicionada como um usuário no ambiente de produção. Para verificar isso, altere a cadeia de conexão a `databaseConnectionStrings.config` de arquivos no banco de dados de desenvolvimento e, em seguida, visite o `Login.aspx` página no ambiente de desenvolvimento. Tente entrar como Sam (consulte **Figura 3**).

[![](users-and-roles-on-the-production-website-cs/_static/image8.png)](users-and-roles-on-the-production-website-cs/_static/image7.png)

**Figura 3**: você não pode entrar como Sam no ambiente de desenvolvimento  
([Clique para exibir a imagem em tamanho normal](users-and-roles-on-the-production-website-cs/_static/image9.png))

Você não pode entrar como Sam no ambiente de desenvolvimento porque as informações de conta de usuário não existe no banco de dados local. Em vez disso, é foi adicionado ao banco de dados de produção. Para verificar isso, exibir o conteúdo do `aspnet_Users` tabela nos bancos de dados de desenvolvimento e produção. No ambiente de desenvolvimento deve ser apenas três registros para os usuários Scott Jisun e Alice. No entanto, o `aspnet_Users` tabela no banco de dados de produção tem quatro registros: Scott, Jisun, Alice e Sam. Consequentemente, Sam pode entrar por meio do site em produção, mas não por meio do ambiente de desenvolvimento.

[![](users-and-roles-on-the-production-website-cs/_static/image11.png)](users-and-roles-on-the-production-website-cs/_static/image10.png)

**Figura 4**: Sam pode entrar no site de produção  
([Clique para exibir a imagem em tamanho normal](users-and-roles-on-the-production-website-cs/_static/image12.png))

> [!NOTE]
> Não se esqueça de alterar a cadeia de conexão no `databaseConnectionStrings.config` arquivo no banco de dados de desenvolvimento da cadeia de conexão quando você terminar trabalhando com WSAT caso contrário, você estará trabalhando com dados de produção ao testar o site por meio do desenvolvimento ambiente. Tenha em mente que, enquanto a técnica que acabamos de discutir permite usar o WSAT para gerenciar remotamente funções e usuários, as alterações feitas as outras opções de configuração de WSAT (regras de acesso, SMTP configurações, depuração e rastreamento configurações e assim por diante) modificar a também`Web.config` arquivo. Consequentemente, as alterações feitas nas configurações se aplicam ao ambiente de desenvolvimento e não para o ambiente de produção.


## <a name="creating-custom-user-and-role-management-web-pages"></a>Criação de usuário personalizada e páginas de Web de gerenciamento de função

O WSAT fornece um fora da caixa de sistema para gerenciar usuários e funções, mas só pode ser iniciado localmente e exige alterações às informações de cadeia de caracteres de conexão para gerenciar os usuários e funções na produção. A maioria dos sites que dão suporte a contas de usuário também incluem um número de páginas de web de administração de função que permitem aos administradores gerenciar usuários e funções de páginas dentro do site e de usuário. Essas páginas de administração baseado na web facilitam gerenciar usuários e funções e são essenciais para sites onde pode haver muitos administradores ou administradores que não têm acesso ou o plano de fundo técnico para usar o Visual Studio para iniciar o WSAT.

O ASP.NET inclui uma série de controles da Web relacionadas a logon internos que tornam a implementação de muitas dessas páginas da web administrativas tão fácil quanto arrastar e soltar. Por exemplo, você pode criar uma página para administradores criar uma nova conta de usuário arrastando o controle CreateUserWizard para a página e definir algumas propriedades. Na verdade, a página de criação de usuários no WSAT mostrado na **Figura 2** usa o controle CreateUserWizard mesmo que você pode adicionar às páginas. Além disso, as funcionalidades de associação e funções dos serviços estão disponíveis programaticamente por meio de `Membership` e `Roles` classes no .NET Framework. Com essas classes, você pode escrever código para criar, editar e excluir usuários e funções, bem como para adicionar ou remover usuários das funções, para determinar quais usuários estão em que funções e executar outras tarefas relacionadas ao usuário e à função.

No [ *Configurando um site que usa serviços de aplicativos* tutorial](configuring-a-website-that-uses-application-services-cs.md) adicionei uma página para o `Admin` pasta denominada `CreateAccount.aspx`. Esta página permite que um administrador para adicionar uma nova conta de usuário para o site e especificar se é ou não o usuário recém-criado na função de administrador (consulte **Figura 5**).

[![](users-and-roles-on-the-production-website-cs/_static/image14.png)](users-and-roles-on-the-production-website-cs/_static/image13.png)

**Figura 5**: os administradores podem criar novas contas de usuário  
([Clique para exibir a imagem em tamanho normal](users-and-roles-on-the-production-website-cs/_static/image15.png))

Para obter informações mais detalhada sobre criar páginas de administração de usuário e a função, junto com instruções passo a passo sobre como usar o `Membership` e `Roles` classes e os controles da Web do ASP.NET relacionadas a logon, certifique-se de ler meu [a segurança de site Tutoriais](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md). Lá você encontrará orientações sobre como criar páginas da web para a criação de novas contas, criar e gerenciar funções, atribuir usuários a funções e outras tarefas administrativas comuns.

Para implementar a funcionalidade como WSAT no site de produção, você pode criar sempre sua série de páginas da web que implementam os recursos do WSAT. Para ajudar a começar, verifique o código-fonte WSAT, que está localizado na pasta `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\ASP.NETWebAdminFiles`. Outra opção é usar a alternativa WSAT de Dan Clem, que ele compartilha em seu artigo, [sem interrupção seu próprio Site da ferramenta de administração](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx). Dan orienta leitores durante o processo de criação de uma ferramenta semelhante WSAT personalizada, inclui o código-fonte do seu aplicativo para download (em c#) e fornece instruções passo a passo para adicionar sua WSAT personalizado a um site hospedado.

## <a name="summary"></a>Resumo

A ferramenta de administração de Site da Web do ASP.NET (WSAT) pode ser usado em conjunto com os serviços de aplicativo de associação e funções para gerenciar informações de usuário e a função para seu site. Infelizmente, o WSAT é acessível apenas localmente e não pode ser visitada do seu site de produção. No entanto, alterando a cadeia de caracteres de conexão no desenvolvimento de ambiente para apontar para o banco de dados de produção, você pode usar o WSAT para gerenciar os usuários e funções no site de produção.

Enquanto a abordagem WSAT proporciona uma maneira rápida e fácil de gerenciar usuários e funções, ele exige iniciando WSAT do Visual Studio, bem como alterações temporárias para as informações de cadeia de caracteres de conexão. O WSAT oferece uma maneira rápida de gerenciar usuários e funções na produção, mas é difícil e não funcionam bem para sites com vários administradores ou administradores que não têm ou não estiver familiarizado com o Visual Studio e o WSAT. Por esses motivos, a maioria dos sites que dão suporte a contas de usuário incluem um conjunto de páginas da web administrativas. Esse conjunto de páginas da web elimina a necessidade do WSAT e usado por vários usuários administrativos, de qualquer computador.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Examinando ASP. Associação, funções e perfil do NET](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Implantar sua própria ferramenta de administração de Site da Web](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [Visão geral de ferramenta de administração de Site da Web](https://msdn.microsoft.com/en-us/library/yy40ytx0.aspx)
- [Tutoriais de segurança de site](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)

>[!div class="step-by-step"]
[Anterior](precompiling-your-website-cs.md)
[Próximo](asp-net-hosting-options-vb.md)
