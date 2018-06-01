---
title: Configuração de logon externo com o ASP.NET Core do Twitter
author: rick-anderson
description: Este tutorial demonstra a integração da autenticação de usuário de conta do Twitter em um aplicativo existente do ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 11/01/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/twitter-logins
ms.openlocfilehash: 3f59f7d1bf0280cef8f7757e8cd57d4872769b3d
ms.sourcegitcommit: 545ff5a632e2281035c1becec1f99137298e4f5c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2018
ms.locfileid: "34688990"
---
# <a name="twitter-external-login-setup-with-aspnet-core"></a>Configuração de logon externo com o ASP.NET Core do Twitter

Por [Valeriy Novytskyy](https://github.com/01binary) e [Rick Anderson](https://twitter.com/RickAndMSFT)

Este tutorial mostra como habilitar usuários para [entrar com sua conta do Twitter](https://dev.twitter.com/web/sign-in/desktop-browser) usando um projeto do ASP.NET Core 2.0 de exemplo criado na [página anterior](xref:security/authentication/social/index).

## <a name="create-the-app-in-twitter"></a>Criar o aplicativo no Twitter

* Navegue até [ https://apps.twitter.com/ ](https://apps.twitter.com/) e entrar. Se você ainda não tiver uma conta do Twitter, use o **[inscrever-se agora](https://twitter.com/signup)** link para criar uma. Depois de entrar, o **gerenciamento de aplicativos** página é mostrada:

![Abra o gerenciamento de aplicativos no Microsoft Edge do Twitter](index/_static/TwitterAppManage.png)

* Toque em **criar novo aplicativo** e preencha o aplicativo **nome**, **descrição** pública e **site** URI (que pode ser temporária até que você Registre o nome de domínio):

![Criar uma página de aplicativo](index/_static/TwitterCreate.png)

* Insira o URI de desenvolvimento com */signin-twitter* acrescentados no **válido URIs de redirecionamento OAuth** campo (por exemplo: `https://localhost:44320/signin-twitter`). O esquema de autenticação do Twitter configurado posteriormente neste tutorial automaticamente manipulará as solicitações no */signin-twitter* rota para implementar o fluxo do OAuth.

* Preencha o restante do formulário e toque em **criar seu aplicativo Twitter**. Novos detalhes do aplicativo são exibidos:

![Guia de detalhes na página de aplicativo](index/_static/TwitterAppDetails.png)

* Ao implantar o site será necessário rever o **gerenciamento de aplicativos** página e registrar um novo URI público.

## <a name="storing-twitter-consumerkey-and-consumersecret"></a>Armazenar Twitter ConsumerKey e ConsumerSecret

Vincular as configurações confidenciais, como o Twitter `Consumer Key` e `Consumer Secret` para sua configuração de aplicativo usando o [Manager segredo](xref:security/app-secrets). Para os fins deste tutorial, nomeie os tokens `Authentication:Twitter:ConsumerKey` e `Authentication:Twitter:ConsumerSecret`.

Esses tokens podem ser encontradas no **chaves e Tokens de acesso** guia depois de criar seu novo aplicativo Twitter:

![Guia de chaves e Tokens de acesso](index/_static/TwitterKeys.png)

## <a name="configure-twitter-authentication"></a>Configurar a autenticação do Twitter

O modelo de projeto usado neste tutorial garante que [Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter) pacote já está instalado.

* Para instalar este pacote com o Visual Studio 2017, clique com botão direito no projeto e selecione **gerenciar pacotes NuGet**.
* Para instalar o .NET Core CLI, execute o seguinte comando no diretório do projeto:

   `dotnet add package Microsoft.AspNetCore.Authentication.Twitter`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

Adicione o serviço do Twitter no `ConfigureServices` método *Startup.cs* arquivo:

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddTwitter(twitterOptions =>
{
    twitterOptions.ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"];
    twitterOptions.ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](~/includes/chain-auth-providers.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Adicionar middleware do Twitter no `Configure` método *Startup.cs* arquivo:

```csharp
app.UseTwitterAuthentication(new TwitterOptions()
{
    ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"],
    ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"]
});
```

---

Consulte o [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) referência de API para obter mais informações sobre opções de configuração com suporte pela autenticação do Twitter. Isso pode ser usado para solicitar informações diferentes sobre o usuário.

## <a name="sign-in-with-twitter"></a>Entrar com o Twitter

Execute o aplicativo e clique em **login**. Será exibida uma opção para entrar com o Twitter:

![Aplicativo Web: usuário não autenticado](index/_static/DoneTwitter.png)

Clicando em **Twitter** redireciona para o Twitter para autenticação:

![Página de autenticação do Twitter](index/_static/TwitterLogin.png)

Depois de inserir suas credenciais do Twitter, você será redirecionado para o site onde você pode definir seu email.

Agora você está conectado usando suas credenciais do Twitter:

![Aplicativo Web: usuário autenticado](index/_static/Done.png)

## <a name="troubleshooting"></a>Solução de problemas

* Apenas **ASP.NET Core 2.x:** se a identidade não for configurada chamando `services.AddIdentity` no `ConfigureServices`, a autenticação resultará em *ArgumentException: a opção 'SignInScheme' deve ser fornecida*. O modelo de projeto usado neste tutorial garante que isso é feito.
* Se o banco de dados do site não tiver sido criado, aplicando a migração inicial, você obterá *uma operação de banco de dados falhou ao processar a solicitação* erro. Toque em **aplicar migrações** para criar o banco de dados e a atualização para continuar após o erro.

## <a name="next-steps"></a>Próximas etapas

* Este artigo mostrou como você pode autenticar com o Twitter. Você pode seguir uma abordagem semelhante para autenticar com outros provedores listados no [página anterior](xref:security/authentication/social/index).

* Depois de publicar seu site da web para o aplicativo web do Azure, você deve redefinir o `ConsumerSecret` no portal do desenvolvedor do Twitter.

* Definir o `Authentication:Twitter:ConsumerKey` e `Authentication:Twitter:ConsumerSecret` como configurações de aplicativo no portal do Azure. O sistema de configuração é configurado para ler as chaves de variáveis de ambiente.
