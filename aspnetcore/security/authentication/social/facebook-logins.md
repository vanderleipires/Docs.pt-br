---
title: Configuração de logon externo do Facebook no ASP.NET Core
author: rick-anderson
description: Este tutorial demonstra a integração da autenticação de usuário de conta do Facebook em um aplicativo ASP.NET Core existente.
ms.author: riande
ms.date: 08/01/2017
uid: security/authentication/facebook-logins
ms.openlocfilehash: 3ba6fe7785afa268e54e6032f1963c1867f6bb27
ms.sourcegitcommit: 74c09caec8992635825b45b7f065f871d33c077a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/22/2018
ms.locfileid: "42634803"
---
# <a name="facebook-external-login-setup-in-aspnet-core"></a><span data-ttu-id="3fb1b-103">Configuração de logon externo do Facebook no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3fb1b-103">Facebook external login setup in ASP.NET Core</span></span>

<span data-ttu-id="3fb1b-104">Por [Valeriy Novytskyy](https://github.com/01binary) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="3fb1b-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="3fb1b-105">Este tutorial mostra como permitir que os usuários entrem com a conta do Facebook deles usando um projeto de amostra do ASP.NET Core 2.0 criado na [página anterior](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="3fb1b-105">This tutorial shows you how to enable your users to sign in with their Facebook account using a sample ASP.NET Core 2.0 project created on the [previous page](xref:security/authentication/social/index).</span></span> <span data-ttu-id="3fb1b-106">Autenticação do Facebook requer o [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) pacote do NuGet.</span><span class="sxs-lookup"><span data-stu-id="3fb1b-106">Facebook authentication requires the [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) NuGet package.</span></span> <span data-ttu-id="3fb1b-107">Vamos começar criando um Facebook App ID seguindo as [etapas oficiais](https://developers.facebook.com).</span><span class="sxs-lookup"><span data-stu-id="3fb1b-107">We start by creating a Facebook App ID by following the [official steps](https://developers.facebook.com).</span></span>

## <a name="create-the-app-in-facebook"></a><span data-ttu-id="3fb1b-108">Criar o aplicativo no Facebook</span><span class="sxs-lookup"><span data-stu-id="3fb1b-108">Create the app in Facebook</span></span>

* <span data-ttu-id="3fb1b-109">Navegue até a página [aplicativos no site para desenvolvedores do Facebook](https://developers.facebook.com/apps/).</span><span class="sxs-lookup"><span data-stu-id="3fb1b-109">Navigate to the [Facebook Developers app](https://developers.facebook.com/apps/) page and sign in.</span></span> <span data-ttu-id="3fb1b-110">Se você ainda não tiver uma conta do Facebook, use o link **Sign up foinscrever-se para o Facebook** na página de logon para criar uma.</span><span class="sxs-lookup"><span data-stu-id="3fb1b-110">If you don't already have a Facebook account, use the **Sign up for Facebook** link on the login page to create one.</span></span>

* <span data-ttu-id="3fb1b-111">Clique no botão **adicionar um novo aplicativo** no canto superior direito para criar uma nova ID de aplicativo.</span><span class="sxs-lookup"><span data-stu-id="3fb1b-111">Tap the **Add a New App** button in the upper right corner to create a new App ID.</span></span>

   ![Abra o Facebook para o portal de desenvolvedores no Microsoft Edge](index/_static/FBMyApps.png)

* <span data-ttu-id="3fb1b-113">Preencha o formulário e clique no botão **criar ID do aplicativo**.</span><span class="sxs-lookup"><span data-stu-id="3fb1b-113">Fill out the form and tap the **Create App ID** button.</span></span>

   ![Criar um formulário de nova ID do aplicativo](index/_static/FBNewAppId.png)

* <span data-ttu-id="3fb1b-115">Na página **selecionar um produto** , clique em **Set Up** no painel **Facebook Login**.</span><span class="sxs-lookup"><span data-stu-id="3fb1b-115">On the **Select a product** page, click **Set Up** on the **Facebook Login** card.</span></span>

   ![Página de instalação do produto](index/_static/FBProductSetup.png)

* <span data-ttu-id="3fb1b-117">O assistente **Quickstart** iniciará com **escolher uma plataforma** como a primeira página.</span><span class="sxs-lookup"><span data-stu-id="3fb1b-117">The **Quickstart** wizard will launch with **Choose a Platform** as the first page.</span></span> <span data-ttu-id="3fb1b-118">Ignore o assistente clicando no link **configurações** no menu à esquerda:</span><span class="sxs-lookup"><span data-stu-id="3fb1b-118">Bypass the wizard for now by clicking the **Settings** link in the menu on the left:</span></span>

   ![Início rápido do Skip](index/_static/FBSkipQuickStart.png)

* <span data-ttu-id="3fb1b-120">Você verá a **configurações do cliente OAuth** página:</span><span class="sxs-lookup"><span data-stu-id="3fb1b-120">You are presented with the **Client OAuth Settings** page:</span></span>

![Página de configurações do OAuth do cliente](index/_static/FBOAuthSetup.png)

* <span data-ttu-id="3fb1b-122">Insira o URI de desenvolvimento com */signin-facebook* acrescentado para o **URIs de redirecionamento de OAuth válido** campo (por exemplo: `https://localhost:44320/signin-facebook`).</span><span class="sxs-lookup"><span data-stu-id="3fb1b-122">Enter your development URI with */signin-facebook* appended into the **Valid OAuth Redirect URIs** field (for example: `https://localhost:44320/signin-facebook`).</span></span> <span data-ttu-id="3fb1b-123">A autenticação do Facebook configurada mais tarde neste tutorial automaticamente manipulará as solicitações no */signin-facebook* rota para implementar o fluxo de OAuth.</span><span class="sxs-lookup"><span data-stu-id="3fb1b-123">The Facebook authentication configured later in this tutorial will automatically handle requests at */signin-facebook* route to implement the OAuth flow.</span></span>

> [!NOTE]
> <span data-ttu-id="3fb1b-124">O URI */signin-facebook* é definido como o retorno de chamada padrão do provedor de autenticação do Facebook.</span><span class="sxs-lookup"><span data-stu-id="3fb1b-124">The URI */signin-facebook* is set as the default callback of the Facebook authentication provider.</span></span> <span data-ttu-id="3fb1b-125">Você pode alterar o retorno de chamada padrão URI ao configurar o middleware de autenticação do Facebook por meio de herdadas [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) propriedade da [FacebookOptions](/dotnet/api/microsoft.aspnetcore.authentication.facebook.facebookoptions) classe.</span><span class="sxs-lookup"><span data-stu-id="3fb1b-125">You can change the default callback URI while configuring the Facebook authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [FacebookOptions](/dotnet/api/microsoft.aspnetcore.authentication.facebook.facebookoptions) class.</span></span>

* <span data-ttu-id="3fb1b-126">Clique em **salvar alterações**.</span><span class="sxs-lookup"><span data-stu-id="3fb1b-126">Click **Save Changes**.</span></span>

* <span data-ttu-id="3fb1b-127">Clique em **Configurações > básico** link no painel de navegação à esquerda.</span><span class="sxs-lookup"><span data-stu-id="3fb1b-127">Click **Settings > Basic** link in the left navigation.</span></span> 

    <span data-ttu-id="3fb1b-128">Nessa página, anote seu `App ID` e seu `App Secret`.</span><span class="sxs-lookup"><span data-stu-id="3fb1b-128">On this page, make a note of your `App ID` and your `App Secret`.</span></span> <span data-ttu-id="3fb1b-129">Você adicionará os dois em seu aplicativo ASP.NET Core na próxima seção:</span><span class="sxs-lookup"><span data-stu-id="3fb1b-129">You will add both into your ASP.NET Core application in the next section:</span></span>


* <span data-ttu-id="3fb1b-130">Ao implantar o site, você precisará voltar para a página de configurações **Logon do Facebook** e registrar um novo URI público.</span><span class="sxs-lookup"><span data-stu-id="3fb1b-130">When deploying the site you need to revisit the **Facebook Login** setup page and register a new public URI.</span></span>

## <a name="store-facebook-app-id-and-app-secret"></a><span data-ttu-id="3fb1b-131">ID do Facebook App Store e o segredo do aplicativo</span><span class="sxs-lookup"><span data-stu-id="3fb1b-131">Store Facebook App ID and App Secret</span></span>

<span data-ttu-id="3fb1b-132">Vincular as configurações confidenciais, como o Facebook `App ID` e `App Secret` para sua configuração de aplicativo usando o [Secret Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="3fb1b-132">Link sensitive settings like Facebook `App ID` and `App Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="3fb1b-133">Para os fins deste tutorial, nomeie os tokens `Authentication:Facebook:AppId` e `Authentication:Facebook:AppSecret`.</span><span class="sxs-lookup"><span data-stu-id="3fb1b-133">For the purposes of this tutorial, name the tokens `Authentication:Facebook:AppId` and `Authentication:Facebook:AppSecret`.</span></span>

<span data-ttu-id="3fb1b-134">Execute os seguintes comandos para armazenar com segurança `App ID` e `App Secret` usando o Secret Manager:</span><span class="sxs-lookup"><span data-stu-id="3fb1b-134">Execute the following commands to securely store `App ID` and `App Secret` using Secret Manager:</span></span>

```console
dotnet user-secrets set Authentication:Facebook:AppId <app-id>
dotnet user-secrets set Authentication:Facebook:AppSecret <app-secret>
```

## <a name="configure-facebook-authentication"></a><span data-ttu-id="3fb1b-135">Configurar a autenticação do Facebook</span><span class="sxs-lookup"><span data-stu-id="3fb1b-135">Configure Facebook Authentication</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3fb1b-136">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3fb1b-136">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="3fb1b-137">Adicione o serviço do Facebook no método `ConfigureServices` do arquivo *Startup.cs* :</span><span class="sxs-lookup"><span data-stu-id="3fb1b-137">Add the Facebook service in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3fb1b-138">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3fb1b-138">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="3fb1b-139">Instalar o pacote [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook).</span><span class="sxs-lookup"><span data-stu-id="3fb1b-139">Install the [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) package.</span></span>

* <span data-ttu-id="3fb1b-140">Para instalar este pacote com o Visual Studio 2017, clique com botão direito no projeto e selecione **gerenciar pacotes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="3fb1b-140">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="3fb1b-141">Para instalar o .NET Core CLI, execute o seguinte comando no diretório do projeto:</span><span class="sxs-lookup"><span data-stu-id="3fb1b-141">To install with .NET Core CLI, execute the following in your project directory:</span></span>

   `dotnet add package Microsoft.AspNetCore.Authentication.Facebook`

<span data-ttu-id="3fb1b-142">Adicione o middleware do Facebook no método `Configure` do arquivo *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="3fb1b-142">Add the Facebook middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseFacebookAuthentication(new FacebookOptions()
{
    AppId = Configuration["Authentication:Facebook:AppId"],
    AppSecret = Configuration["Authentication:Facebook:AppSecret"]
});
```

---

<span data-ttu-id="3fb1b-143">Consulte as referências de API do [FacebookOptions](/dotnet/api/microsoft.aspnetcore.builder.facebookoptions) para obter mais informações sobre as opções de configuração compatíveis com a autenticação do Facebook.</span><span class="sxs-lookup"><span data-stu-id="3fb1b-143">See the [FacebookOptions](/dotnet/api/microsoft.aspnetcore.builder.facebookoptions) API reference for more information on configuration options supported by Facebook authentication.</span></span> <span data-ttu-id="3fb1b-144">As opções de configuração podem ser usadas para:</span><span class="sxs-lookup"><span data-stu-id="3fb1b-144">Configuration options can be used to:</span></span>

* <span data-ttu-id="3fb1b-145">Solicitar informações diferentes sobre o usuário.</span><span class="sxs-lookup"><span data-stu-id="3fb1b-145">Request different information about the user.</span></span>
* <span data-ttu-id="3fb1b-146">Adicionar argumentos de cadeia de caracteres de consulta para personalizar a experiência de logon.</span><span class="sxs-lookup"><span data-stu-id="3fb1b-146">Add query string arguments to customize the login experience.</span></span>

## <a name="sign-in-with-facebook"></a><span data-ttu-id="3fb1b-147">Entrar com o Facebook</span><span class="sxs-lookup"><span data-stu-id="3fb1b-147">Sign in with Facebook</span></span>

<span data-ttu-id="3fb1b-148">Executar o aplicativo e clique em **faça logon no**.</span><span class="sxs-lookup"><span data-stu-id="3fb1b-148">Run your application and click **Log in**.</span></span> <span data-ttu-id="3fb1b-149">Você verá uma opção para entrar com o Facebook.</span><span class="sxs-lookup"><span data-stu-id="3fb1b-149">You see an option to sign in with Facebook.</span></span>

![Aplicativo Web: usuário não autenticado](index/_static/DoneFacebook.png)

<span data-ttu-id="3fb1b-151">Quando você clica em **Facebook**, você será redirecionado ao Facebook para autenticação:</span><span class="sxs-lookup"><span data-stu-id="3fb1b-151">When you click on **Facebook**, you are redirected to Facebook for authentication:</span></span>

![Página de autenticação do Facebook](index/_static/FBLogin.png)

<span data-ttu-id="3fb1b-153">Autenticação do Facebook solicita o endereço de email e o perfil público por padrão:</span><span class="sxs-lookup"><span data-stu-id="3fb1b-153">Facebook authentication requests public profile and email address by default:</span></span>

![Página de autenticação do Facebook](index/_static/FBLoginDone.png)

<span data-ttu-id="3fb1b-155">Depois que você insira suas credenciais do Facebook, você será redirecionado para seu site em que você pode definir seu email.</span><span class="sxs-lookup"><span data-stu-id="3fb1b-155">Once you enter your Facebook credentials you are redirected back to your site where you can set your email.</span></span>

<span data-ttu-id="3fb1b-156">Agora você está conectado usando suas credenciais do Facebook:</span><span class="sxs-lookup"><span data-stu-id="3fb1b-156">You are now logged in using your Facebook credentials:</span></span>

![Aplicativo Web: usuário autenticado](index/_static/Done.png)

## <a name="troubleshooting"></a><span data-ttu-id="3fb1b-158">Solução de problemas</span><span class="sxs-lookup"><span data-stu-id="3fb1b-158">Troubleshooting</span></span>

* <span data-ttu-id="3fb1b-159">Apenas **ASP.NET Core 2.x:** se a identidade não for configurada chamando `services.AddIdentity` no `ConfigureServices`, a autenticação resultará em *ArgumentException: a opção 'SignInScheme' deve ser fornecida*.</span><span class="sxs-lookup"><span data-stu-id="3fb1b-159">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="3fb1b-160">O modelo de projeto usado neste tutorial garante que isso é feito.</span><span class="sxs-lookup"><span data-stu-id="3fb1b-160">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="3fb1b-161">Se o banco de dados do site não foi criado por meio da aplicação a migração inicial, você obterá *uma operação de banco de dados falhou ao processar a solicitação* erro.</span><span class="sxs-lookup"><span data-stu-id="3fb1b-161">If the site database has not been created by applying the initial migration, you get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="3fb1b-162">Toque **aplicar migrações** para criar o banco de dados e atualizar para continuar após o erro.</span><span class="sxs-lookup"><span data-stu-id="3fb1b-162">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3fb1b-163">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="3fb1b-163">Next steps</span></span>

* <span data-ttu-id="3fb1b-164">Este artigo mostrou como você pode autenticar com o Facebook.</span><span class="sxs-lookup"><span data-stu-id="3fb1b-164">This article showed how you can authenticate with Facebook.</span></span> <span data-ttu-id="3fb1b-165">Você pode seguir uma abordagem semelhante para autenticar com outros provedores listados na [página anterior](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="3fb1b-165">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="3fb1b-166">Depois de publicar seu site da web para aplicativo web do Azure, você deve redefinir o `AppSecret` no portal do desenvolvedor do Facebook.</span><span class="sxs-lookup"><span data-stu-id="3fb1b-166">Once you publish your web site to Azure web app, you should reset the `AppSecret` in the Facebook developer portal.</span></span>

* <span data-ttu-id="3fb1b-167">Defina as `Authentication:Facebook:AppId` e `Authentication:Facebook:AppSecret` como configurações de aplicativo no portal do Azure.</span><span class="sxs-lookup"><span data-stu-id="3fb1b-167">Set the `Authentication:Facebook:AppId` and `Authentication:Facebook:AppSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="3fb1b-168">Configurar o sistema de configuração para ler as chaves de variáveis de ambiente.</span><span class="sxs-lookup"><span data-stu-id="3fb1b-168">The configuration system is set up to read keys from environment variables.</span></span>
