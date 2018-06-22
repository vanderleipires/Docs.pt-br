---
title: Pesquisa rápida de outros provedores de autenticação
author: rick-anderson
ms.author: riande
ms.date: 11/03/2016
uid: security/authentication/otherlogins
ms.openlocfilehash: 9c2ce02f4613fddbe0e767724019d80ac056bf7b
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274047"
---
# <a name="short-survey-of-other-authentication-providers"></a>Pesquisa rápida de outros provedores de autenticação

<a name="security-authentication-other-logins"></a>

Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Pranav Rastogi](https://github.com/rustd), e [Valeriy Novytskyy](https://github.com/01binary)

Aqui estão configurados instruções para alguns outros provedores OAuth comuns. Pacotes do NuGet de terceiros, como aqueles mantidos pelo [aspnet Contribuidor](https://www.nuget.org/packages?q=owners%3Aaspnet-contrib+title%3AOAuth) pode ser usada para complementar os provedores de autenticação implementados pela equipe do ASP.NET Core.

* Configurar **LinkedIn** entrar: [ https://www.linkedin.com/developer/apps ](https://www.linkedin.com/developer/apps). Consulte [etapas oficiais](https://developer.linkedin.com/docs/oauth2).

* Configurar **Instagram** entrar: [ https://www.instagram.com/developer/register/ ](https://www.instagram.com/developer/register/). Consulte [etapas oficiais](https://www.instagram.com/developer/authentication/).

* Configurar **Reddit** entrar: [ https://www.reddit.com/login?dest=https%3A%2F%2Fwww.reddit.com%2Fprefs%2Fapps ](https://www.reddit.com/login?dest=https%3A%2F%2Fwww.reddit.com%2Fprefs%2Fapps). Consulte [etapas oficiais](https://github.com/reddit/reddit/wiki/OAuth2-Quick-Start-Example).

* Configurar **Github** entrar: [ https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2Fsettings%2Fapplications%2Fnew ](https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2Fsettings%2Fapplications%2Fnew). Consulte [etapas oficiais](https://developer.github.com/v3/oauth/).

* Configurar **Yahoo** entrar: [ https://login.yahoo.com/config/login?src=devnet&.done=http%3A%2F%2Fdeveloper.yahoo.com%2Fapps%2Fcreate%2F ](https://login.yahoo.com/config/login?src=devnet&.done=http%3A%2F%2Fdeveloper.yahoo.com%2Fapps%2Fcreate%2F). Consulte [etapas oficiais](https://developer.yahoo.com/bbauth/user.html).

* Configurar **Tumblr** entrar: [ https://www.tumblr.com/oauth/apps ](https://www.tumblr.com/oauth/apps). Consulte [etapas oficiais](https://www.tumblr.com/docs/api/v2#auth).

* Configurar **Pinterest** entrar: [ https://www.pinterest.com/login/?next=http%3A%2F%2Fdevsite%2Fapps%2F ](https://www.pinterest.com/login/?next=http%3A%2F%2Fdevsite%2Fapps%2F). Consulte [etapas oficiais](https://developers.pinterest.com/docs/api/overview/?).

* Configurar **Pocket** entrar: [ https://getpocket.com/developer/apps/new ](https://getpocket.com/developer/apps/new). Consulte [etapas oficiais](https://getpocket.com/developer/docs/authentication).

* Configurar **Flickr** entrar: [ https://www.flickr.com/services/apps/create ](https://www.flickr.com/services/apps/create). Consulte [etapas oficiais](https://www.flickr.com/services/api/auth.oauth.html).

* Configurar **Dribble** entrar: [ https://dribbble.com/signup ](https://dribbble.com/signup). Consulte [etapas oficiais](http://developer.dribbble.com/v1/oauth/).

* Configurar **Vimeo** entrar: [ https://vimeo.com/join ](https://vimeo.com/join). Consulte [etapas oficiais](https://developer.vimeo.com/api/authentication).

* Configurar **SoundCloud** entrar: [ https://soundcloud.com/you/apps/new ](https://soundcloud.com/you/apps/new). Consulte [etapas oficiais](https://developers.soundcloud.com/blog/we-love-oauth-2).

* Configurar **VK** entrar: [ https://vk.com/apps?act=manage ](https://vk.com/apps?act=manage). Consulte [etapas oficiais](https://vk.com/pages?oid=-17680044&p=Authorizing_Sites).

## <a name="multiple-authentication-providers"></a>Vários provedores de autenticação

[!INCLUDE[](~/includes/chain-auth-providers.md)]
