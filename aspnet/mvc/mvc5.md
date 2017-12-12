---
uid: mvc/mvc5
title: O ASP.NET MVC 5 | Microsoft Docs
author: rick-anderson
description: "O ASP.NET MVC 5 ASP.NET MVC 5 é uma estrutura para criar aplicativos web escalonável, baseado em padrões usando padrões de design bem estabelecidos e a potência de AS...."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: f79fbf7f-59e5-4279-a832-c1a0294630f4
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/mvc5
msc.type: content
ms.openlocfilehash: e57163469ae4606df0fc17e3e054b7696782a084
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-5"></a>ASP.NET MVC 5
====================
## <a name="whats-new-in-aspnet-mvc-5"></a>O que há de novo no ASP.NET MVC 5

### <a name="one-aspnet"></a>Um ASP.NET

Os modelos de projeto da Web MVC integração perfeita com a nova experiência One ASP.NET. Você pode personalizar seu projeto MVC e configurar a autenticação usando o Assistente de criação do ASP.NET de um projeto. Um tutorial de Introdução ao ASP.NET MVC 5 pode ser encontrado em [guia de Introdução ao ASP.NET MVC 5](overview/getting-started/introduction/getting-started.md).

Para obter informações sobre como atualizar projetos MVC 4 a 5 do MVC, consulte [como atualizar um ASP.NET MVC 4 e o projeto de API da Web para ASP.NET MVC 5 e Web API 2](overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).

### <a name="aspnet-identity"></a>ASP.NET Identity

Os modelos de projeto MVC foram atualizados para usar a identidade do ASP.NET para autenticação e gerenciamento de identidade. Um tutorial com a autenticação do Facebook e do Google e a nova API de associação pode ser encontrado em [criar um aplicativo do ASP.NET MVC 5 com o Facebook e Google OAuth2 e Sign-on OpenID](overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) e [implantar um aplicativo de proteger o ASP.NET MVC com Associação, OAuth e o banco de dados SQL para um Site da Web do Azure Windows](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).

### <a name="bootstrap"></a>inicialização

O modelo de projeto MVC foi atualizado para usar [Bootstrap](http://getbootstrap.com/) para fornecer uma elegante e ágeis aparência que você pode personalizar facilmente. Para obter mais informações, consulte [Bootstrap em modelos de projeto da web do Visual Studio 2013](../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap).

### <a name="authentication-filters"></a>Filtros de autenticação

[Filtros de autenticação](http://www.dotnetcurry.com/showarticle.aspx?ID=957) são um novo tipo de filtro no ASP.NET MVC que executar antes de filtros de autorização no pipeline do ASP.NET MVC e permitem que você especifique autenticação lógica por ação, por controlador, ou globalmente para todos os controladores. Filtros de autenticação processam as credenciais na solicitação e fornecem uma entidade correspondente. Filtros de autenticação também podem adicionar os desafios de autenticação em resposta a solicitações não autorizadas. Consulte [filtros de autenticação do ASP.NET MVC 5](http://www.dotnetcurry.com/showarticle.aspx?ID=957), [filtros de autenticação no ASP.NET MVC 5](http://theshravan.net/blog/authentication-filters-in-asp-net-mvc-5/) e [finalmente novo ASP.NET MVC 5 filtros de autenticação!](http://hackwebwith.net/finally-the-new-asp-net-mvc-5-authentication-filters/).

### <a name="filter-overrides"></a>Substituições de filtro

Você agora pode substituir os filtros se aplicam a um controlador ou método de ação em questão, especificando um [substituir filtro](http://www.davidhayden.me/blog/filter-overrides-in-asp-net-mvc-5). Os filtros de substituição de especificar um conjunto de tipos de filtro não deve ser executado para um determinado escopo (ação ou controlador). Isso permite que você configurar os filtros que se aplicam globalmente, mas, em seguida, excluir determinados filtros globais da aplicação para ações específicas ou controladores. Consulte [substituições de filtro novo recurso no ASP.NET MVC 5 e ASP.NET Web API 2](https://weblogs.asp.net/imranbaloch/archive/2013/09/25/new-filter-overrides-in-asp-net-mvc-5-and-asp-net-web-api-2.aspx), [como usar o recurso de substituições de filtro do ASP.NET MVC 5](http://hackwebwith.net/how-to-use-the-asp-net-mvc-5-filter-overrides-feature/), e [filtro substituições no ASP.NET MVC 5](http://www.davidhayden.me/blog/filter-overrides-in-asp-net-mvc-5)

### <a name="attribute-routing"></a>Roteamento de atributo

ASP.NET MVC agora dá suporte a [roteamento de atributo](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx), graças uma contribuição por Tim McCall, o autor do [http://attributerouting.net](http://attributerouting.net). Com o roteamento de atributo, você pode especificar suas rotas, anotando ações e controladores.

## <a name="new-web-project-experience"></a>Nova experiência de projeto da Web

É aprimorada a experiência de criação de novos projetos da web no Visual Studio 2013. No **novo projeto da Web ASP.NET** caixa de diálogo que você pode selecionar o tipo de projeto você deseja, configurar qualquer combinação de tecnologias (Web Forms, MVC, Web API), configurar opções de autenticação e adicionar um projeto de teste de unidade.

![Novo projeto ASP.NET](mvc5/_static/image1.png)

A nova caixa de diálogo permite que você altere as opções de autenticação padrão para muitos dos modelos. Por exemplo, quando você cria um projeto ASP.NET Web Forms você pode selecionar qualquer uma das seguintes opções:

- Sem autenticação
- Contas de usuário individuais (associação ASP.NET ou sociais do provedor de log no)
- Contas organizacionais (Active Directory em um aplicativo de internet)
- Autenticação do Windows (Active Directory em um aplicativo de intranet)

![Opções de autenticação](mvc5/_static/image2.png)

Para obter mais informações sobre o novo processo para a criação de projetos da web, consulte [criar projetos da Web do ASP.NET no Visual Studio 2013](../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md). Para obter mais informações sobre as novas opções de autenticação, consulte [ASP.NET Identity](../identity/overview/index.md).

<a id="scaffold"></a>
### <a name="aspnet-scaffolding"></a>Estrutura do ASP.NET

Estrutura do ASP.NET é uma estrutura de geração de código para aplicativos Web ASP.NET. Torna mais fácil adicionar código clichê ao seu projeto que interage com um modelo de dados.

Nas versões anteriores do Visual Studio, scaffolding foi limitado a projetos do ASP.NET MVC. Com o Visual Studio 2013, agora você pode usar o scaffolding para qualquer projeto ASP.NET, incluindo formulários da Web. Visual Studio 2013 não oferece suporte para gerar páginas para um projeto de Web Forms, mas você ainda pode usar o scaffolding com formulários da Web Adicionando dependências MVC ao projeto. O suporte para a geração de páginas para formulários da Web será adicionado em uma atualização futura.

Ao usar o scaffolding, podemos garantir que todos os dependências estejam instaladas no projeto. Por exemplo, se você iniciar com um projeto Web Forms do ASP.NET e, em seguida, use o scaffolding para adicionar um controlador de API da Web, os pacotes do NuGet necessários e as referências são adicionadas ao seu projeto automaticamente.

Para adicionar o scaffolding MVC para um projeto de Web Forms, adicione um **Novo Item de Scaffold** e selecione **MVC 5 dependências** na janela de diálogo. Há duas opções para a estrutura MVC; Mínimo e completo. Se você selecionar no mínimo, somente os pacotes NuGet e referências para o ASP.NET MVC são adicionadas ao seu projeto. Se você selecionar a opção completo, as dependências mínimo são adicionadas, bem como os arquivos de conteúdo necessários para um projeto MVC.

Suporte para a estrutura async controladores usa os novos recursos de assíncronas do Entity Framework 6.

Para obter mais informações e tutoriais, consulte [visão geral do ASP.NET Scaffolding](../visual-studio/overview/2013/aspnet-scaffolding-overview.md).

### <a name="getting-help-and-reporting-issues"></a>Obtendo ajuda e relatar problemas

- [Problemas conhecidos e lista de alterações de quebra](../visual-studio/overview/2013/release-notes.md#knownissues)
- Obter ajuda e discutir o ASP.NET MVC 5 a [fóruns](https://forums.asp.net/1146.aspx)
- [Relatar um erro no ASP.NET MVC 5](https://github.com/aspnet/AspNetWebStack/issues)
- [Fazer uma solicitação de recurso](http://aspnet.uservoice.com/forums/41201-asp-net-mvc)

### <a name="upgrading-from-aspnet-mvc-4"></a>Atualização do ASP.NET MVC 4

Consulte [como atualizar um ASP.NET MVC 4 e projeto de API para o ASP.NET MVC 5 e Web API 2 da Web](overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md)
