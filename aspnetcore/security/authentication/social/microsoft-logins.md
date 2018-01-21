---
title: "O programa de instalação do Microsoft Account logon externo"
author: rick-anderson
description: "Este tutorial demonstra a integração da autenticação de usuário de conta da Microsoft em um aplicativo existente do ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 08/24/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/microsoft-logins
ms.openlocfilehash: 6e4586eb681bd230413ace67ca9eddc3fe3e9e60
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/19/2018
---
# <a name="configuring-microsoft-account-authentication"></a>Configurar a autenticação do Microsoft Account

Por [Valeriy Novytskyy](https://github.com/01binary) e [Rick Anderson](https://twitter.com/RickAndMSFT)

Este tutorial mostra como habilitar os usuários entrar com sua conta da Microsoft usando um projeto do ASP.NET Core 2.0 de exemplo criado no [página anterior](index.md).

## <a name="create-the-app-in-microsoft-developer-portal"></a>Criar o aplicativo no Portal do desenvolvedor da Microsoft

* Navegue até [https://apps.dev.microsoft.com](https://apps.dev.microsoft.com) e criar ou entrar em uma conta da Microsoft:

![Entrar na caixa de diálogo](index/_static/MicrosoftDevLogin.png)

Se você ainda não tiver uma conta da Microsoft, toque em  **[criar um!](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=13&ct=1478151035&rver=6.7.6643.0&wp=SAPI_LONG&wreply=https%3a%2f%2fapps.dev.microsoft.com%2fLoginPostBack&id=293053&aadredir=1&contextid=D70D4F21246BAB50&bk=1478151036&uiflavor=web&uaid=f0c3de863a914c358b8dc01b1ff49e85&mkt=EN-US&lc=1033&lic=1)** Depois de entrar, você será redirecionado para **meus aplicativos** página:

![Portal do desenvolvedor do Microsoft aberto no Microsoft Edge](index/_static/MicrosoftDev.png)

* Toque em **adicionar um aplicativo** no canto superior direito de canto e insira seu **nome do aplicativo** e **Contact Email**:

![Caixa de diálogo Nova registro de aplicativo](index/_static/MicrosoftDevAppCreate.png)

* Para os fins deste tutorial, desmarque o **instalação interativa** caixa de seleção.

* Toque em **criar** para continuar a **registro** página. Forneça um **nome** e observe o valor da **Id do aplicativo**, que você usar como `ClientId` posteriormente no tutorial:

![Página de registro](index/_static/MicrosoftDevAppReg.png)

* Toque em **Adicionar plataforma** no **plataformas** seção e selecione o **Web** plataforma:

![Adicionar caixa de diálogo de plataforma](index/_static/MicrosoftDevAppPlatform.png)

* No novo **Web** plataforma seção, digite a URL de desenvolvimento com */signin-microsoft* acrescentados no **URLs de redirecionamento** campo (por exemplo: `https://localhost:44320/signin-microsoft`). O esquema de autenticação da Microsoft configurado mais tarde neste tutorial automaticamente manipulará as solicitações no */signin-microsoft* rota para implementar o fluxo de OAuth:

![Seção de plataforma da Web](index/_static/MicrosoftRedirectUri.png)

* Toque em **Adicionar URL** para garantir que a URL foi adicionada.

* Preencha quaisquer outras configurações de aplicativo, se necessário e toque em **salvar** na parte inferior da página para salvar as alterações de configuração do aplicativo.

* Ao implantar o site será necessário rever o **registro** página e defina uma nova URL pública.

## <a name="store-microsoft-application-id-and-password"></a>Armazenar a Id do aplicativo da Microsoft e a senha

* Observe o `Application Id` exibido no **registro** página.

* Toque em **gerar nova senha** no **segredos do aplicativo** seção. Isso exibe uma caixa em que você pode copiar a senha de aplicativo:

![Caixa de diálogo Nova senha gerada](index/_static/MicrosoftDevPassword.png)

Vincular as configurações confidenciais como Microsoft `Application ID` e `Password` para sua configuração de aplicativo usando o [Manager segredo](../../app-secrets.md). Para os fins deste tutorial, nomeie os tokens `Authentication:Microsoft:ApplicationId` e `Authentication:Microsoft:Password`.

## <a name="configure-microsoft-account-authentication"></a>Configurar a autenticação de conta da Microsoft

O modelo de projeto usado neste tutorial garante que [Microsoft.AspNetCore.Authentication.MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount) pacote já está instalado.

* Para instalar este pacote com 2017 do Visual Studio, clique com botão direito no projeto e selecione **gerenciar pacotes NuGet**.
* Para instalar o .NET Core CLI, execute o seguinte no diretório do projeto:

   `dotnet add package Microsoft.AspNetCore.Authentication.MicrosoftAccount`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Adicione o serviço Microsoft Account no `ConfigureServices` método *Startup.cs* arquivo:

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddMicrosoftAccount(microsoftOptions =>
{
    microsoftOptions.ClientId = Configuration["Authentication:Microsoft:ApplicationId"];
    microsoftOptions.ClientSecret = Configuration["Authentication:Microsoft:Password"];
});
```

[!INCLUDE[default settings configuration](includes/default-settings.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Adicionar o middleware Account da Microsoft no `Configure` método *Startup.cs* arquivo:

```csharp
app.UseMicrosoftAccountAuthentication(new MicrosoftAccountOptions()
{
    ClientId = Configuration["Authentication:Microsoft:ApplicationId"],
    ClientSecret = Configuration["Authentication:Microsoft:Password"]
});
```

---

Embora a terminologia usada no Portal do desenvolvedor do Microsoft nomes esses tokens `ApplicationId` e `Password`, eles são expostos como `ClientId` e `ClientSecret` para a API de configuração.

Consulte o [MicrosoftAccountOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.microsoftaccountoptions) referência de API para obter mais informações sobre opções de configuração com suporte pela autenticação Account da Microsoft. Isso pode ser usado para solicitar informações diferentes sobre o usuário.

## <a name="sign-in-with-microsoft-account"></a>Entrar com a conta da Microsoft

Execute o aplicativo e clique em **login**. Uma opção para entrar com Microsoft será exibida:

![Log de aplicativo na página da Web: usuário não autenticado](index/_static/DoneMicrosoft.png)

Quando você clica na Microsoft, você é redirecionado para a Microsoft para autenticação. Depois de entrar com sua Account da Microsoft (se ainda não estiver conectado), você será solicitado para permitir que o aplicativo acessar suas informações:

![Caixa de diálogo de autenticação Microsoft](index/_static/MicrosoftLogin.png)

Toque em **Sim** e você será redirecionado para o site da web onde você pode definir seu email.

Agora você está conectado usando suas credenciais da Microsoft:

![Aplicativo Web: usuário autenticado](index/_static/Done.png)

## <a name="troubleshooting"></a>Solução de problemas

* Se o provedor do Microsoft Account redireciona para uma página de erro de entrada, observe os erro título e descrição de cadeia de caracteres parâmetros de consulta diretamente após o `#` (hashtag) no Uri.

  Embora a mensagem de erro parece ser um problema com a autenticação do Microsoft, a causa mais comum é seu aplicativo Uri não corresponde a nenhuma do **URIs de redirecionamento** especificado para o **Web** plataforma .
* **ASP.NET Core 2. x somente:** identidade se não está configurada por meio da chamada `services.AddIdentity` na `ConfigureServices`, tentar autenticar resultará em *ArgumentException: A opção 'SignInScheme' deve ser fornecida*. O modelo de projeto usado neste tutorial garante que isso é feito.
* Se o banco de dados do site não tiver sido criado, aplicando a migração inicial, você obterá *uma operação de banco de dados falhou ao processar a solicitação* erro. Toque em **aplicar migrações** para criar o banco de dados e a atualização para continuar após o erro.

## <a name="next-steps"></a>Próximas etapas

* Este artigo mostrou como você pode autenticar com a Microsoft. Você pode seguir uma abordagem semelhante para autenticar com outros provedores listados no [página anterior](index.md).

* Depois de publicar seu site da web para o aplicativo web do Azure, você deve criar um novo `Password` no Portal do desenvolvedor do Microsoft.

* Definir o `Authentication:Microsoft:ApplicationId` e `Authentication:Microsoft:Password` como configurações de aplicativo no portal do Azure. O sistema de configuração é configurado para ler as chaves de variáveis de ambiente.
