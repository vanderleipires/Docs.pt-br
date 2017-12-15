---
title: "Configuração de logon externo do Facebook no núcleo do ASP.NET"
author: rick-anderson
description: "Este tutorial demonstra a integração da autenticação de usuário de conta do Facebook em um aplicativo existente do ASP.NET Core."
keywords: "ASP.NET Core, Facebook, login, autenticação"
ms.author: riande
manager: wpickett
ms.date: 08/01/2017
ms.topic: article
ms.assetid: 8c65179b-688c-4af1-8f5e-1862920cda95
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/facebook-logins
ms.openlocfilehash: 058670b4f699288e1acbe76bae08dcebf69346b8
ms.sourcegitcommit: 198fb0488e961048bfa376cf58cb853ef1d1cb91
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/14/2017
---
# <a name="configuring-facebook-authentication"></a><span data-ttu-id="251b9-104">Configurar a autenticação do Facebook</span><span class="sxs-lookup"><span data-stu-id="251b9-104">Configuring Facebook authentication</span></span>

<span data-ttu-id="251b9-105">Por [Valeriy Novytskyy](https://github.com/01binary) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="251b9-105">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="251b9-106">Este tutorial mostra como habilitar os usuários entrar com sua conta do Facebook usando um projeto do ASP.NET Core 2.0 de exemplo criado no [página anterior](index.md).</span><span class="sxs-lookup"><span data-stu-id="251b9-106">This tutorial shows you how to enable your users to sign in with their Facebook account using a sample ASP.NET Core 2.0 project created on the [previous page](index.md).</span></span> <span data-ttu-id="251b9-107">Vamos começar criando um Facebook App ID seguindo o [etapas oficiais](https://developers.facebook.com).</span><span class="sxs-lookup"><span data-stu-id="251b9-107">We start by creating a Facebook App ID by following the [official steps](https://developers.facebook.com).</span></span>

## <a name="create-the-app-in-facebook"></a><span data-ttu-id="251b9-108">Criar o aplicativo no Facebook</span><span class="sxs-lookup"><span data-stu-id="251b9-108">Create the app in Facebook</span></span>

*  <span data-ttu-id="251b9-109">Navegue até o [aplicativo Facebook desenvolvedores](https://developers.facebook.com/apps/) página e entre.</span><span class="sxs-lookup"><span data-stu-id="251b9-109">Navigate to the [Facebook Developers app](https://developers.facebook.com/apps/) page and sign in.</span></span> <span data-ttu-id="251b9-110">Se você ainda não tiver uma conta do Facebook, use o **inscrever-se para o Facebook** link na página de logon para criar uma.</span><span class="sxs-lookup"><span data-stu-id="251b9-110">If you don't already have a Facebook account, use the **Sign up for Facebook** link on the login page to create one.</span></span>

* <span data-ttu-id="251b9-111">Toque na **adicionar um novo aplicativo** botão no canto superior direito para criar uma nova ID de aplicativo.</span><span class="sxs-lookup"><span data-stu-id="251b9-111">Tap the **Add a New App** button in the upper right corner to create a new App ID.</span></span>

   ![Abra o Facebook para o portal de desenvolvedores no Microsoft Edge](index/_static/FBMyApps.png)

* <span data-ttu-id="251b9-113">Preencha o formulário e toque no **criar ID do aplicativo** botão.</span><span class="sxs-lookup"><span data-stu-id="251b9-113">Fill out the form and tap the **Create App ID** button.</span></span>

   ![Criar um formulário de nova ID de aplicativo](index/_static/FBNewAppId.png)

* <span data-ttu-id="251b9-115">No **selecionar um produto** , clique em **Set Up** no **logon do Facebook** cartão.</span><span class="sxs-lookup"><span data-stu-id="251b9-115">On the **Select a product** page, click **Set Up** on the **Facebook Login** card.</span></span>

   ![Página de instalação do produto](index/_static/FBProductSetup.png)
  
* <span data-ttu-id="251b9-117">O **Quickstart** assistente iniciará com **escolher uma plataforma** como a primeira página.</span><span class="sxs-lookup"><span data-stu-id="251b9-117">The **Quickstart** wizard will launch with **Choose a Platform** as the first page.</span></span> <span data-ttu-id="251b9-118">Ignorar o assistente agora clicando o **configurações** link no menu à esquerda:</span><span class="sxs-lookup"><span data-stu-id="251b9-118">Bypass the wizard for now by clicking the **Settings** link in the menu on the left:</span></span>

   ![Início rápido do Skip](index/_static/FBSkipQuickStart.png)

* <span data-ttu-id="251b9-120">Você verá o **configurações do cliente OAuth** página:</span><span class="sxs-lookup"><span data-stu-id="251b9-120">You are presented with the **Client OAuth Settings** page:</span></span>

![Página de configurações de OAuth do cliente](index/_static/FBOAuthSetup.png)

* <span data-ttu-id="251b9-122">Insira o URI de desenvolvimento com */signin-facebook* acrescentados no **válido URIs de redirecionamento OAuth** campo (por exemplo: `https://localhost:44320/signin-facebook`).</span><span class="sxs-lookup"><span data-stu-id="251b9-122">Enter your development URI with */signin-facebook* appended into the **Valid OAuth Redirect URIs** field (for example: `https://localhost:44320/signin-facebook`).</span></span> <span data-ttu-id="251b9-123">A autenticação do Facebook configurada mais tarde neste tutorial automaticamente manipulará as solicitações no */signin-facebook* rota para implementar o fluxo do OAuth.</span><span class="sxs-lookup"><span data-stu-id="251b9-123">The Facebook authentication configured later in this tutorial will automatically handle requests at */signin-facebook* route to implement the OAuth flow.</span></span>

* <span data-ttu-id="251b9-124">Clique em **salvar alterações**.</span><span class="sxs-lookup"><span data-stu-id="251b9-124">Click **Save Changes**.</span></span>

* <span data-ttu-id="251b9-125">Clique o **painel** link no painel de navegação esquerdo.</span><span class="sxs-lookup"><span data-stu-id="251b9-125">Click the **Dashboard** link in the left navigation.</span></span> 

    <span data-ttu-id="251b9-126">Nessa página, anote o `App ID` e `App Secret`.</span><span class="sxs-lookup"><span data-stu-id="251b9-126">On this page, make a note of your `App ID` and your `App Secret`.</span></span> <span data-ttu-id="251b9-127">Você adicionará ambos em seu aplicativo ASP.NET Core na próxima seção:</span><span class="sxs-lookup"><span data-stu-id="251b9-127">You will add both into your ASP.NET Core application in the next section:</span></span>

   ![Painel do desenvolvedor do Facebook](index/_static/FBDashboard.png)

* <span data-ttu-id="251b9-129">Ao implantar o site que você precise revisá o **logon do Facebook** página de instalação e registrar um novo URI público.</span><span class="sxs-lookup"><span data-stu-id="251b9-129">When deploying the site you need to revisit the **Facebook Login** setup page and register a new public URI.</span></span>

## <a name="store-facebook-app-id-and-app-secret"></a><span data-ttu-id="251b9-130">Armazenar a ID do aplicativo Facebook e o segredo do aplicativo</span><span class="sxs-lookup"><span data-stu-id="251b9-130">Store Facebook App ID and App Secret</span></span>

<span data-ttu-id="251b9-131">Vincular as configurações confidenciais como Facebook `App ID` e `App Secret` para sua configuração de aplicativo usando o [Manager segredo](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="251b9-131">Link sensitive settings like Facebook `App ID` and `App Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="251b9-132">Para os fins deste tutorial, nomeie os tokens `Authentication:Facebook:AppId` e `Authentication:Facebook:AppSecret`.</span><span class="sxs-lookup"><span data-stu-id="251b9-132">For the purposes of this tutorial, name the tokens `Authentication:Facebook:AppId` and `Authentication:Facebook:AppSecret`.</span></span>

<span data-ttu-id="251b9-133">Execute os seguintes comandos para armazenar com segurança `App ID` e `App Secret` usando o Gerenciador de segredo:</span><span class="sxs-lookup"><span data-stu-id="251b9-133">Execute the following commands to securely store `App ID` and `App Secret` using Secret Manager:</span></span>

```console
dotnet user-secrets set Authentication:Facebook:AppId <app-id>
dotnet user-secrets set Authentication:Facebook:AppSecret <app-secret>
```

## <a name="configure-facebook-authentication"></a><span data-ttu-id="251b9-134">Configurar a autenticação do Facebook</span><span class="sxs-lookup"><span data-stu-id="251b9-134">Configure Facebook Authentication</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="251b9-135">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="251b9-135">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="251b9-136">Adicione o serviço do Facebook no `ConfigureServices` método o *Startup.cs* arquivo:</span><span class="sxs-lookup"><span data-stu-id="251b9-136">Add the Facebook service in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="251b9-137">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="251b9-137">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="251b9-138">Instalar o [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) pacote.</span><span class="sxs-lookup"><span data-stu-id="251b9-138">Install the [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) package.</span></span>

* <span data-ttu-id="251b9-139">Para instalar este pacote com 2017 do Visual Studio, clique com botão direito no projeto e selecione **gerenciar pacotes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="251b9-139">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="251b9-140">Para instalar o .NET Core CLI, execute o seguinte no diretório do projeto:</span><span class="sxs-lookup"><span data-stu-id="251b9-140">To install with .NET Core CLI, execute the following in your project directory:</span></span>

   `dotnet add package Microsoft.AspNetCore.Authentication.Facebook`

<span data-ttu-id="251b9-141">Adicionar o middleware do Facebook no `Configure` método *Startup.cs* arquivo:</span><span class="sxs-lookup"><span data-stu-id="251b9-141">Add the Facebook middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseFacebookAuthentication(new FacebookOptions()
{
    AppId = Configuration["Authentication:Facebook:AppId"],
    AppSecret = Configuration["Authentication:Facebook:AppSecret"]
});
```

---

<span data-ttu-id="251b9-142">Consulte o [FacebookOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.facebookoptions) referência de API para obter mais informações sobre opções de configuração com suporte a autenticação do Facebook.</span><span class="sxs-lookup"><span data-stu-id="251b9-142">See the [FacebookOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.facebookoptions) API reference for more information on configuration options supported by Facebook authentication.</span></span> <span data-ttu-id="251b9-143">Opções de configuração podem ser usadas para:</span><span class="sxs-lookup"><span data-stu-id="251b9-143">Configuration options can be used to:</span></span>

* <span data-ttu-id="251b9-144">Solicite informações diferentes sobre o usuário.</span><span class="sxs-lookup"><span data-stu-id="251b9-144">Request different information about the user.</span></span>
* <span data-ttu-id="251b9-145">Adicione argumentos de cadeia de caracteres de consulta para personalizar a experiência de logon.</span><span class="sxs-lookup"><span data-stu-id="251b9-145">Add query string arguments to customize the login experience.</span></span>

## <a name="sign-in-with-facebook"></a><span data-ttu-id="251b9-146">Entrar com o Facebook</span><span class="sxs-lookup"><span data-stu-id="251b9-146">Sign in with Facebook</span></span>

<span data-ttu-id="251b9-147">Execute o aplicativo e clique em **login**.</span><span class="sxs-lookup"><span data-stu-id="251b9-147">Run your application and click **Log in**.</span></span> <span data-ttu-id="251b9-148">Você verá uma opção para entrar com o Facebook.</span><span class="sxs-lookup"><span data-stu-id="251b9-148">You see an option to sign in with Facebook.</span></span>

![Aplicativo Web: usuário não autenticado](index/_static/DoneFacebook.png)

<span data-ttu-id="251b9-150">Quando você clica na **Facebook**, você será redirecionado para o Facebook para autenticação:</span><span class="sxs-lookup"><span data-stu-id="251b9-150">When you click on **Facebook**, you are redirected to Facebook for authentication:</span></span>

![Página de autenticação do Facebook](index/_static/FBLogin.png)

<span data-ttu-id="251b9-152">Endereço de email e o perfil público de solicitações de autenticação do Facebook por padrão:</span><span class="sxs-lookup"><span data-stu-id="251b9-152">Facebook authentication requests public profile and email address by default:</span></span>

![Página de autenticação do Facebook](index/_static/FBLoginDone.png)

<span data-ttu-id="251b9-154">Depois que você insira suas credenciais de Facebook, que você será redirecionado para o site onde você pode definir seu email.</span><span class="sxs-lookup"><span data-stu-id="251b9-154">Once you enter your Facebook credentials you are redirected back to your site where you can set your email.</span></span>

<span data-ttu-id="251b9-155">Agora você está conectado usando suas credenciais do Facebook:</span><span class="sxs-lookup"><span data-stu-id="251b9-155">You are now logged in using your Facebook credentials:</span></span>

![Aplicativo Web: usuário autenticado](index/_static/Done.png)

## <a name="troubleshooting"></a><span data-ttu-id="251b9-157">Solução de problemas</span><span class="sxs-lookup"><span data-stu-id="251b9-157">Troubleshooting</span></span>

* <span data-ttu-id="251b9-158">**ASP.NET Core 2. x somente:** identidade se não está configurada por meio da chamada `services.AddIdentity` na `ConfigureServices`, tentar autenticar resultará em *ArgumentException: A opção 'SignInScheme' deve ser fornecida*.</span><span class="sxs-lookup"><span data-stu-id="251b9-158">**ASP.NET Core 2.x only:** If Identity is not configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="251b9-159">O modelo de projeto usado neste tutorial garante que isso é feito.</span><span class="sxs-lookup"><span data-stu-id="251b9-159">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="251b9-160">Se o banco de dados do site não tiver sido criado, aplicando a migração inicial, você obtém *uma operação de banco de dados falhou ao processar a solicitação* erro.</span><span class="sxs-lookup"><span data-stu-id="251b9-160">If the site database has not been created by applying the initial migration, you get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="251b9-161">Toque em **aplicar migrações** para criar o banco de dados e a atualização para continuar após o erro.</span><span class="sxs-lookup"><span data-stu-id="251b9-161">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="251b9-162">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="251b9-162">Next steps</span></span>

* <span data-ttu-id="251b9-163">Este artigo mostrou como você pode autenticar com o Facebook.</span><span class="sxs-lookup"><span data-stu-id="251b9-163">This article showed how you can authenticate with Facebook.</span></span> <span data-ttu-id="251b9-164">Você pode seguir uma abordagem semelhante para autenticar com outros provedores listados no [página anterior](index.md).</span><span class="sxs-lookup"><span data-stu-id="251b9-164">You can follow a similar approach to authenticate with other providers listed on the [previous page](index.md).</span></span>

* <span data-ttu-id="251b9-165">Depois de publicar seu site da web para o aplicativo web do Azure, você deve redefinir o `AppSecret` no portal do desenvolvedor do Facebook.</span><span class="sxs-lookup"><span data-stu-id="251b9-165">Once you publish your web site to Azure web app, you should reset the `AppSecret` in the Facebook developer portal.</span></span>

* <span data-ttu-id="251b9-166">Definir o `Authentication:Facebook:AppId` e `Authentication:Facebook:AppSecret` como configurações de aplicativo no portal do Azure.</span><span class="sxs-lookup"><span data-stu-id="251b9-166">Set the `Authentication:Facebook:AppId` and `Authentication:Facebook:AppSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="251b9-167">O sistema de configuração é configurado para ler as chaves de variáveis de ambiente.</span><span class="sxs-lookup"><span data-stu-id="251b9-167">The configuration system is set up to read keys from environment variables.</span></span>
