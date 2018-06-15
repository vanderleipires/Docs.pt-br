---
title: Configuração de logon externo do Facebook no núcleo do ASP.NET
author: rick-anderson
description: Este tutorial demonstra a integração da autenticação de usuário de conta do Facebook em um aplicativo existente do ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 08/01/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/facebook-logins
ms.openlocfilehash: f9c28930c1f8a9c54792a2f689d890f16d795a55
ms.sourcegitcommit: 4e3497bda0c3e5011ffba3717eb61a1d46c61c15
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/14/2018
ms.locfileid: "35613103"
---
# <a name="facebook-external-login-setup-in-aspnet-core"></a>Configuração de logon externo do Facebook no núcleo do ASP.NET

Por [Valeriy Novytskyy](https://github.com/01binary) e [Rick Anderson](https://twitter.com/RickAndMSFT)

Este tutorial mostra como permitir que os usuários entrem com a conta do Facebook deles usando um projeto de amostra do ASP.NET Core 2.0 criado na [página anterior](xref:security/authentication/social/index). Requer a autenticação do Facebook do [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) pacote NuGet. Vamos começar criando um Facebook App ID seguindo as [etapas oficiais](https://developers.facebook.com).

## <a name="create-the-app-in-facebook"></a>Criar o aplicativo no Facebook

* Navegue até a página [aplicativos no site para desenvolvedores do Facebook](https://developers.facebook.com/apps/). Se você ainda não tiver uma conta do Facebook, use o link **Sign up foinscrever-se para o Facebook** na página de logon para criar uma.

* Clique no botão **adicionar um novo aplicativo** no canto superior direito para criar uma nova ID de aplicativo.

   ![Abra o Facebook para o portal de desenvolvedores no Microsoft Edge](index/_static/FBMyApps.png)

* Preencha o formulário e clique no botão **criar ID do aplicativo**.

   ![Criar um formulário de nova ID de aplicativo](index/_static/FBNewAppId.png)

* Na página **selecionar um produto** , clique em **Set Up** no painel **Facebook Login**.

   ![Página de instalação do produto](index/_static/FBProductSetup.png)

* O assistente **Quickstart** iniciará com **escolher uma plataforma** como a primeira página. Ignore o assistente clicando no link **configurações** no menu à esquerda:

   ![Início rápido do Skip](index/_static/FBSkipQuickStart.png)

* Você verá o **configurações do cliente OAuth** página:

![Página de configurações de OAuth do cliente](index/_static/FBOAuthSetup.png)

* Insira o URI de desenvolvimento com */signin-facebook* acrescentados no **válido URIs de redirecionamento OAuth** campo (por exemplo: `https://localhost:44320/signin-facebook`). A autenticação do Facebook configurada mais tarde neste tutorial automaticamente manipulará as solicitações no */signin-facebook* rota para implementar o fluxo do OAuth.

> [!NOTE]
> O URI */signin-facebook* é definido como o retorno de chamada padrão do provedor de autenticação do Facebook. Você pode alterar o retorno de chamada padrão URI ao configurar o middleware de autenticação do Facebook através de herdadas [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) propriedade o [FacebookOptions](/dotnet/api/microsoft.aspnetcore.authentication.facebook.facebookoptions) classe.

* Clique em **salvar alterações**.

* Clique o **painel** link no painel de navegação esquerdo. 

    Nessa página, anote o `App ID` e `App Secret`. Você adicionará ambos em seu aplicativo ASP.NET Core na próxima seção:

   ![Painel do desenvolvedor do Facebook](index/_static/FBDashboard.png)

* Ao implantar o site, você precisará voltar para a página de configurações **Logon do Facebook** e registrar um novo URI público.

## <a name="store-facebook-app-id-and-app-secret"></a>Armazenar a ID do aplicativo Facebook e o segredo do aplicativo

Vincular as configurações confidenciais como Facebook `App ID` e `App Secret` para sua configuração de aplicativo usando o [Manager segredo](xref:security/app-secrets). Para os fins deste tutorial, nomeie os tokens `Authentication:Facebook:AppId` e `Authentication:Facebook:AppSecret`.

Execute os seguintes comandos para armazenar com segurança `App ID` e `App Secret` usando o Gerenciador de segredo:

```console
dotnet user-secrets set Authentication:Facebook:AppId <app-id>
dotnet user-secrets set Authentication:Facebook:AppSecret <app-secret>
```

## <a name="configure-facebook-authentication"></a>Configurar a autenticação do Facebook

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

Adicione o serviço do Facebook no método `ConfigureServices` do arquivo *Startup.cs* :

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddFacebook(facebookOptions =>
{
    facebookOptions.AppId = Configuration["Authentication:Facebook:AppId"];
    facebookOptions.AppSecret = Configuration["Authentication:Facebook:AppSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](~/includes/chain-auth-providers.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Instalar o pacote [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook).

* Para instalar este pacote com o Visual Studio 2017, clique com botão direito no projeto e selecione **gerenciar pacotes NuGet**.
* Para instalar o .NET Core CLI, execute o seguinte comando no diretório do projeto:

   `dotnet add package Microsoft.AspNetCore.Authentication.Facebook`

Adicione o middleware do Facebook no método `Configure` do arquivo *Startup.cs*:

```csharp
app.UseFacebookAuthentication(new FacebookOptions()
{
    AppId = Configuration["Authentication:Facebook:AppId"],
    AppSecret = Configuration["Authentication:Facebook:AppSecret"]
});
```

---

Consulte as referências de API do [FacebookOptions](/dotnet/api/microsoft.aspnetcore.builder.facebookoptions) para obter mais informações sobre as opções de configuração compatíveis com a autenticação do Facebook. As opções de configuração podem ser usadas para:

* Solicitar informações diferentes sobre o usuário.
* Adicionar argumentos de cadeia de caracteres de consulta para personalizar a experiência de logon.

## <a name="sign-in-with-facebook"></a>Entrar com o Facebook

Execute o aplicativo e clique em **login**. Você verá uma opção para entrar com o Facebook.

![Aplicativo Web: usuário não autenticado](index/_static/DoneFacebook.png)

Quando você clica na **Facebook**, você será redirecionado para o Facebook para autenticação:

![Página de autenticação do Facebook](index/_static/FBLogin.png)

Endereço de email e o perfil público de solicitações de autenticação do Facebook por padrão:

![Página de autenticação do Facebook](index/_static/FBLoginDone.png)

Depois que você insira suas credenciais de Facebook, que você será redirecionado para o site onde você pode definir seu email.

Agora você está conectado usando suas credenciais do Facebook:

![Aplicativo Web: usuário autenticado](index/_static/Done.png)

## <a name="troubleshooting"></a>Solução de problemas

* Apenas **ASP.NET Core 2.x:** se a identidade não for configurada chamando `services.AddIdentity` no `ConfigureServices`, a autenticação resultará em *ArgumentException: a opção 'SignInScheme' deve ser fornecida*. O modelo de projeto usado neste tutorial garante que isso é feito.
* Se o banco de dados do site não tiver sido criado, aplicando a migração inicial, você obtém *uma operação de banco de dados falhou ao processar a solicitação* erro. Toque em **aplicar migrações** para criar o banco de dados e a atualização para continuar após o erro.

## <a name="next-steps"></a>Próximas etapas

* Este artigo mostrou como você pode autenticar com o Facebook. Você pode seguir uma abordagem semelhante para autenticar com outros provedores listados no [página anterior](xref:security/authentication/social/index).

* Depois de publicar seu site da web para o aplicativo web do Azure, você deve redefinir o `AppSecret` no portal do desenvolvedor do Facebook.

* Definir o `Authentication:Facebook:AppId` e `Authentication:Facebook:AppSecret` como configurações de aplicativo no portal do Azure. O sistema de configuração é configurado para ler as chaves de variáveis de ambiente.
