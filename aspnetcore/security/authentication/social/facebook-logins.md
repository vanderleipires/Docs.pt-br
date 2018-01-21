---
title: "Configuração de logon externo do Facebook no núcleo do ASP.NET"
author: rick-anderson
description: "Este tutorial demonstra a integração da autenticação de usuário de conta do Facebook em um aplicativo existente do ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 08/01/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/facebook-logins
ms.openlocfilehash: 2d36aa78f82b4a52a7c6a152bee2c4ca9923409f
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/19/2018
---
# <a name="configuring-facebook-authentication"></a><span data-ttu-id="fa1fd-103">Configurar a autenticação do Facebook</span><span class="sxs-lookup"><span data-stu-id="fa1fd-103">Configuring Facebook authentication</span></span>

<span data-ttu-id="fa1fd-104">Por [Valeriy Novytskyy](https://github.com/01binary) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="fa1fd-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="fa1fd-105">Este tutorial mostra como habilitar os usuários entrar com sua conta do Facebook usando um projeto do ASP.NET Core 2.0 de exemplo criado no [página anterior](index.md).</span><span class="sxs-lookup"><span data-stu-id="fa1fd-105">This tutorial shows you how to enable your users to sign in with their Facebook account using a sample ASP.NET Core 2.0 project created on the [previous page](index.md).</span></span> <span data-ttu-id="fa1fd-106">Vamos começar criando um Facebook App ID seguindo o [etapas oficiais](https://developers.facebook.com).</span><span class="sxs-lookup"><span data-stu-id="fa1fd-106">We start by creating a Facebook App ID by following the [official steps](https://developers.facebook.com).</span></span>

## <a name="create-the-app-in-facebook"></a><span data-ttu-id="fa1fd-107">Criar o aplicativo no Facebook</span><span class="sxs-lookup"><span data-stu-id="fa1fd-107">Create the app in Facebook</span></span>

*  <span data-ttu-id="fa1fd-108">Navegue até o [aplicativo Facebook desenvolvedores](https://developers.facebook.com/apps/) página e entre.</span><span class="sxs-lookup"><span data-stu-id="fa1fd-108">Navigate to the [Facebook Developers app](https://developers.facebook.com/apps/) page and sign in.</span></span> <span data-ttu-id="fa1fd-109">Se você ainda não tiver uma conta do Facebook, use o **inscrever-se para o Facebook** link na página de logon para criar uma.</span><span class="sxs-lookup"><span data-stu-id="fa1fd-109">If you don't already have a Facebook account, use the **Sign up for Facebook** link on the login page to create one.</span></span>

* <span data-ttu-id="fa1fd-110">Toque na **adicionar um novo aplicativo** botão no canto superior direito para criar uma nova ID de aplicativo.</span><span class="sxs-lookup"><span data-stu-id="fa1fd-110">Tap the **Add a New App** button in the upper right corner to create a new App ID.</span></span>

   ![Abra o Facebook para o portal de desenvolvedores no Microsoft Edge](index/_static/FBMyApps.png)

* <span data-ttu-id="fa1fd-112">Preencha o formulário e toque no **criar ID do aplicativo** botão.</span><span class="sxs-lookup"><span data-stu-id="fa1fd-112">Fill out the form and tap the **Create App ID** button.</span></span>

   ![Criar um formulário de nova ID de aplicativo](index/_static/FBNewAppId.png)

* <span data-ttu-id="fa1fd-114">No **selecionar um produto** , clique em **Set Up** no **logon do Facebook** cartão.</span><span class="sxs-lookup"><span data-stu-id="fa1fd-114">On the **Select a product** page, click **Set Up** on the **Facebook Login** card.</span></span>

   ![Página de instalação do produto](index/_static/FBProductSetup.png)
  
* <span data-ttu-id="fa1fd-116">O **Quickstart** assistente iniciará com **escolher uma plataforma** como a primeira página.</span><span class="sxs-lookup"><span data-stu-id="fa1fd-116">The **Quickstart** wizard will launch with **Choose a Platform** as the first page.</span></span> <span data-ttu-id="fa1fd-117">Ignorar o assistente agora clicando o **configurações** link no menu à esquerda:</span><span class="sxs-lookup"><span data-stu-id="fa1fd-117">Bypass the wizard for now by clicking the **Settings** link in the menu on the left:</span></span>

   ![Início rápido do Skip](index/_static/FBSkipQuickStart.png)

* <span data-ttu-id="fa1fd-119">Você verá o **configurações do cliente OAuth** página:</span><span class="sxs-lookup"><span data-stu-id="fa1fd-119">You are presented with the **Client OAuth Settings** page:</span></span>

![Página de configurações de OAuth do cliente](index/_static/FBOAuthSetup.png)

* <span data-ttu-id="fa1fd-121">Insira o URI de desenvolvimento com */signin-facebook* acrescentados no **válido URIs de redirecionamento OAuth** campo (por exemplo: `https://localhost:44320/signin-facebook`).</span><span class="sxs-lookup"><span data-stu-id="fa1fd-121">Enter your development URI with */signin-facebook* appended into the **Valid OAuth Redirect URIs** field (for example: `https://localhost:44320/signin-facebook`).</span></span> <span data-ttu-id="fa1fd-122">A autenticação do Facebook configurada mais tarde neste tutorial automaticamente manipulará as solicitações no */signin-facebook* rota para implementar o fluxo do OAuth.</span><span class="sxs-lookup"><span data-stu-id="fa1fd-122">The Facebook authentication configured later in this tutorial will automatically handle requests at */signin-facebook* route to implement the OAuth flow.</span></span>

* <span data-ttu-id="fa1fd-123">Clique em **salvar alterações**.</span><span class="sxs-lookup"><span data-stu-id="fa1fd-123">Click **Save Changes**.</span></span>

* <span data-ttu-id="fa1fd-124">Clique o **painel** link no painel de navegação esquerdo.</span><span class="sxs-lookup"><span data-stu-id="fa1fd-124">Click the **Dashboard** link in the left navigation.</span></span> 

    <span data-ttu-id="fa1fd-125">Nessa página, anote o `App ID` e `App Secret`.</span><span class="sxs-lookup"><span data-stu-id="fa1fd-125">On this page, make a note of your `App ID` and your `App Secret`.</span></span> <span data-ttu-id="fa1fd-126">Você adicionará ambos em seu aplicativo ASP.NET Core na próxima seção:</span><span class="sxs-lookup"><span data-stu-id="fa1fd-126">You will add both into your ASP.NET Core application in the next section:</span></span>

   ![Painel do desenvolvedor do Facebook](index/_static/FBDashboard.png)

* <span data-ttu-id="fa1fd-128">Ao implantar o site que você precise revisá o **logon do Facebook** página de instalação e registrar um novo URI público.</span><span class="sxs-lookup"><span data-stu-id="fa1fd-128">When deploying the site you need to revisit the **Facebook Login** setup page and register a new public URI.</span></span>

## <a name="store-facebook-app-id-and-app-secret"></a><span data-ttu-id="fa1fd-129">Armazenar a ID do aplicativo Facebook e o segredo do aplicativo</span><span class="sxs-lookup"><span data-stu-id="fa1fd-129">Store Facebook App ID and App Secret</span></span>

<span data-ttu-id="fa1fd-130">Vincular as configurações confidenciais como Facebook `App ID` e `App Secret` para sua configuração de aplicativo usando o [Manager segredo](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="fa1fd-130">Link sensitive settings like Facebook `App ID` and `App Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="fa1fd-131">Para os fins deste tutorial, nomeie os tokens `Authentication:Facebook:AppId` e `Authentication:Facebook:AppSecret`.</span><span class="sxs-lookup"><span data-stu-id="fa1fd-131">For the purposes of this tutorial, name the tokens `Authentication:Facebook:AppId` and `Authentication:Facebook:AppSecret`.</span></span>

<span data-ttu-id="fa1fd-132">Execute os seguintes comandos para armazenar com segurança `App ID` e `App Secret` usando o Gerenciador de segredo:</span><span class="sxs-lookup"><span data-stu-id="fa1fd-132">Execute the following commands to securely store `App ID` and `App Secret` using Secret Manager:</span></span>

```console
dotnet user-secrets set Authentication:Facebook:AppId <app-id>
dotnet user-secrets set Authentication:Facebook:AppSecret <app-secret>
```

## <a name="configure-facebook-authentication"></a><span data-ttu-id="fa1fd-133">Configurar a autenticação do Facebook</span><span class="sxs-lookup"><span data-stu-id="fa1fd-133">Configure Facebook Authentication</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="fa1fd-134">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="fa1fd-134">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="fa1fd-135">Adicione o serviço do Facebook no `ConfigureServices` método o *Startup.cs* arquivo:</span><span class="sxs-lookup"><span data-stu-id="fa1fd-135">Add the Facebook service in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

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

[!INCLUDE[default settings configuration](includes/default-settings.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="fa1fd-136">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="fa1fd-136">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="fa1fd-137">Instalar o [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) pacote.</span><span class="sxs-lookup"><span data-stu-id="fa1fd-137">Install the [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) package.</span></span>

* <span data-ttu-id="fa1fd-138">Para instalar este pacote com 2017 do Visual Studio, clique com botão direito no projeto e selecione **gerenciar pacotes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="fa1fd-138">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="fa1fd-139">Para instalar o .NET Core CLI, execute o seguinte no diretório do projeto:</span><span class="sxs-lookup"><span data-stu-id="fa1fd-139">To install with .NET Core CLI, execute the following in your project directory:</span></span>

   `dotnet add package Microsoft.AspNetCore.Authentication.Facebook`

<span data-ttu-id="fa1fd-140">Adicionar o middleware do Facebook no `Configure` método *Startup.cs* arquivo:</span><span class="sxs-lookup"><span data-stu-id="fa1fd-140">Add the Facebook middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseFacebookAuthentication(new FacebookOptions()
{
    AppId = Configuration["Authentication:Facebook:AppId"],
    AppSecret = Configuration["Authentication:Facebook:AppSecret"]
});
```

---

<span data-ttu-id="fa1fd-141">Consulte o [FacebookOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.facebookoptions) referência de API para obter mais informações sobre opções de configuração com suporte a autenticação do Facebook.</span><span class="sxs-lookup"><span data-stu-id="fa1fd-141">See the [FacebookOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.facebookoptions) API reference for more information on configuration options supported by Facebook authentication.</span></span> <span data-ttu-id="fa1fd-142">Opções de configuração podem ser usadas para:</span><span class="sxs-lookup"><span data-stu-id="fa1fd-142">Configuration options can be used to:</span></span>

* <span data-ttu-id="fa1fd-143">Solicite informações diferentes sobre o usuário.</span><span class="sxs-lookup"><span data-stu-id="fa1fd-143">Request different information about the user.</span></span>
* <span data-ttu-id="fa1fd-144">Adicione argumentos de cadeia de caracteres de consulta para personalizar a experiência de logon.</span><span class="sxs-lookup"><span data-stu-id="fa1fd-144">Add query string arguments to customize the login experience.</span></span>

## <a name="sign-in-with-facebook"></a><span data-ttu-id="fa1fd-145">Entrar com o Facebook</span><span class="sxs-lookup"><span data-stu-id="fa1fd-145">Sign in with Facebook</span></span>

<span data-ttu-id="fa1fd-146">Execute o aplicativo e clique em **login**.</span><span class="sxs-lookup"><span data-stu-id="fa1fd-146">Run your application and click **Log in**.</span></span> <span data-ttu-id="fa1fd-147">Você verá uma opção para entrar com o Facebook.</span><span class="sxs-lookup"><span data-stu-id="fa1fd-147">You see an option to sign in with Facebook.</span></span>

![Aplicativo Web: usuário não autenticado](index/_static/DoneFacebook.png)

<span data-ttu-id="fa1fd-149">Quando você clica na **Facebook**, você será redirecionado para o Facebook para autenticação:</span><span class="sxs-lookup"><span data-stu-id="fa1fd-149">When you click on **Facebook**, you are redirected to Facebook for authentication:</span></span>

![Página de autenticação do Facebook](index/_static/FBLogin.png)

<span data-ttu-id="fa1fd-151">Endereço de email e o perfil público de solicitações de autenticação do Facebook por padrão:</span><span class="sxs-lookup"><span data-stu-id="fa1fd-151">Facebook authentication requests public profile and email address by default:</span></span>

![Página de autenticação do Facebook](index/_static/FBLoginDone.png)

<span data-ttu-id="fa1fd-153">Depois que você insira suas credenciais de Facebook, que você será redirecionado para o site onde você pode definir seu email.</span><span class="sxs-lookup"><span data-stu-id="fa1fd-153">Once you enter your Facebook credentials you are redirected back to your site where you can set your email.</span></span>

<span data-ttu-id="fa1fd-154">Agora você está conectado usando suas credenciais do Facebook:</span><span class="sxs-lookup"><span data-stu-id="fa1fd-154">You are now logged in using your Facebook credentials:</span></span>

![Aplicativo Web: usuário autenticado](index/_static/Done.png)

## <a name="troubleshooting"></a><span data-ttu-id="fa1fd-156">Solução de problemas</span><span class="sxs-lookup"><span data-stu-id="fa1fd-156">Troubleshooting</span></span>

* <span data-ttu-id="fa1fd-157">**ASP.NET Core 2. x somente:** identidade se não está configurada por meio da chamada `services.AddIdentity` na `ConfigureServices`, tentar autenticar resultará em *ArgumentException: A opção 'SignInScheme' deve ser fornecida*.</span><span class="sxs-lookup"><span data-stu-id="fa1fd-157">**ASP.NET Core 2.x only:** If Identity is not configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="fa1fd-158">O modelo de projeto usado neste tutorial garante que isso é feito.</span><span class="sxs-lookup"><span data-stu-id="fa1fd-158">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="fa1fd-159">Se o banco de dados do site não tiver sido criado, aplicando a migração inicial, você obtém *uma operação de banco de dados falhou ao processar a solicitação* erro.</span><span class="sxs-lookup"><span data-stu-id="fa1fd-159">If the site database has not been created by applying the initial migration, you get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="fa1fd-160">Toque em **aplicar migrações** para criar o banco de dados e a atualização para continuar após o erro.</span><span class="sxs-lookup"><span data-stu-id="fa1fd-160">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fa1fd-161">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="fa1fd-161">Next steps</span></span>

* <span data-ttu-id="fa1fd-162">Este artigo mostrou como você pode autenticar com o Facebook.</span><span class="sxs-lookup"><span data-stu-id="fa1fd-162">This article showed how you can authenticate with Facebook.</span></span> <span data-ttu-id="fa1fd-163">Você pode seguir uma abordagem semelhante para autenticar com outros provedores listados no [página anterior](index.md).</span><span class="sxs-lookup"><span data-stu-id="fa1fd-163">You can follow a similar approach to authenticate with other providers listed on the [previous page](index.md).</span></span>

* <span data-ttu-id="fa1fd-164">Depois de publicar seu site da web para o aplicativo web do Azure, você deve redefinir o `AppSecret` no portal do desenvolvedor do Facebook.</span><span class="sxs-lookup"><span data-stu-id="fa1fd-164">Once you publish your web site to Azure web app, you should reset the `AppSecret` in the Facebook developer portal.</span></span>

* <span data-ttu-id="fa1fd-165">Definir o `Authentication:Facebook:AppId` e `Authentication:Facebook:AppSecret` como configurações de aplicativo no portal do Azure.</span><span class="sxs-lookup"><span data-stu-id="fa1fd-165">Set the `Authentication:Facebook:AppId` and `Authentication:Facebook:AppSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="fa1fd-166">O sistema de configuração é configurado para ler as chaves de variáveis de ambiente.</span><span class="sxs-lookup"><span data-stu-id="fa1fd-166">The configuration system is set up to read keys from environment variables.</span></span>
