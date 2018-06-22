---
title: Configuração de logon externo com o ASP.NET Core do Twitter
author: rick-anderson
description: Este tutorial demonstra a integração da autenticação de usuário de conta do Twitter em um aplicativo existente do ASP.NET Core.
ms.author: riande
ms.date: 11/01/2016
uid: security/authentication/twitter-logins
ms.openlocfilehash: 81c19105e4cda932db3302a5df343322fb85abef
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278486"
---
# <a name="twitter-external-login-setup-with-aspnet-core"></a><span data-ttu-id="483b3-103">Configuração de logon externo com o ASP.NET Core do Twitter</span><span class="sxs-lookup"><span data-stu-id="483b3-103">Twitter external login setup with ASP.NET Core</span></span>

<span data-ttu-id="483b3-104">Por [Valeriy Novytskyy](https://github.com/01binary) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="483b3-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="483b3-105">Este tutorial mostra como habilitar usuários para [entrar com sua conta do Twitter](https://dev.twitter.com/web/sign-in/desktop-browser) usando um projeto do ASP.NET Core 2.0 de exemplo criado na [página anterior](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="483b3-105">This tutorial shows you how to enable your users to [sign in with their Twitter account](https://dev.twitter.com/web/sign-in/desktop-browser) using a sample ASP.NET Core 2.0 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-the-app-in-twitter"></a><span data-ttu-id="483b3-106">Criar o aplicativo no Twitter</span><span class="sxs-lookup"><span data-stu-id="483b3-106">Create the app in Twitter</span></span>

* <span data-ttu-id="483b3-107">Navegue até [ https://apps.twitter.com/ ](https://apps.twitter.com/) e entrar.</span><span class="sxs-lookup"><span data-stu-id="483b3-107">Navigate to [https://apps.twitter.com/](https://apps.twitter.com/) and sign in.</span></span> <span data-ttu-id="483b3-108">Se você ainda não tiver uma conta do Twitter, use o **[inscrever-se agora](https://twitter.com/signup)** link para criar uma.</span><span class="sxs-lookup"><span data-stu-id="483b3-108">If you don't already have a Twitter account, use the **[Sign up now](https://twitter.com/signup)** link to create one.</span></span> <span data-ttu-id="483b3-109">Depois de entrar, o **gerenciamento de aplicativos** página é mostrada:</span><span class="sxs-lookup"><span data-stu-id="483b3-109">After signing in, the **Application Management** page is shown:</span></span>

![Abra o gerenciamento de aplicativos no Microsoft Edge do Twitter](index/_static/TwitterAppManage.png)

* <span data-ttu-id="483b3-111">Toque em **criar novo aplicativo** e preencha o aplicativo **nome**, **descrição** pública e **site** URI (que pode ser temporária até que você Registre o nome de domínio):</span><span class="sxs-lookup"><span data-stu-id="483b3-111">Tap **Create New App** and fill out the application **Name**, **Description** and public **Website** URI (this can be temporary until you register the domain name):</span></span>

![Criar uma página de aplicativo](index/_static/TwitterCreate.png)

* <span data-ttu-id="483b3-113">Insira o URI de desenvolvimento com `/signin-twitter` acrescentados no **válido URIs de redirecionamento OAuth** campo (por exemplo: `https://localhost:44320/signin-twitter`).</span><span class="sxs-lookup"><span data-stu-id="483b3-113">Enter your development URI with `/signin-twitter` appended into the **Valid OAuth Redirect URIs** field (for example: `https://localhost:44320/signin-twitter`).</span></span> <span data-ttu-id="483b3-114">O esquema de autenticação do Twitter configurado posteriormente neste tutorial automaticamente manipulará as solicitações no `/signin-twitter` rota para implementar o fluxo do OAuth.</span><span class="sxs-lookup"><span data-stu-id="483b3-114">The Twitter authentication scheme configured later in this tutorial will automatically handle requests at `/signin-twitter` route to implement the OAuth flow.</span></span>

> [!NOTE]
> <span data-ttu-id="483b3-115">O segmento do URI `/signin-twitter` é definido como o retorno de chamada padrão do provedor de autenticação do Twitter.</span><span class="sxs-lookup"><span data-stu-id="483b3-115">The URI segment `/signin-twitter` is set as the default callback of the Twitter authentication provider.</span></span> <span data-ttu-id="483b3-116">Você pode alterar o retorno de chamada padrão URI ao configurar o middleware de autenticação do Twitter via o herdadas [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) propriedade o [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) classe.</span><span class="sxs-lookup"><span data-stu-id="483b3-116">You can change the default callback URI while configuring the Twitter authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) class.</span></span>

* <span data-ttu-id="483b3-117">Preencha o restante do formulário e toque em **criar seu aplicativo Twitter**.</span><span class="sxs-lookup"><span data-stu-id="483b3-117">Fill out the rest of the form and tap **Create your Twitter application**.</span></span> <span data-ttu-id="483b3-118">Novos detalhes do aplicativo são exibidos:</span><span class="sxs-lookup"><span data-stu-id="483b3-118">New application details are displayed:</span></span>

![Guia de detalhes na página de aplicativo](index/_static/TwitterAppDetails.png)

* <span data-ttu-id="483b3-120">Ao implantar o site será necessário rever o **gerenciamento de aplicativos** página e registrar um novo URI público.</span><span class="sxs-lookup"><span data-stu-id="483b3-120">When deploying the site you'll need to revisit the **Application Management** page and register a new public URI.</span></span>

## <a name="storing-twitter-consumerkey-and-consumersecret"></a><span data-ttu-id="483b3-121">Armazenar Twitter ConsumerKey e ConsumerSecret</span><span class="sxs-lookup"><span data-stu-id="483b3-121">Storing Twitter ConsumerKey and ConsumerSecret</span></span>

<span data-ttu-id="483b3-122">Vincular as configurações confidenciais, como o Twitter `Consumer Key` e `Consumer Secret` para sua configuração de aplicativo usando o [Manager segredo](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="483b3-122">Link sensitive settings like Twitter `Consumer Key` and `Consumer Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="483b3-123">Para os fins deste tutorial, nomeie os tokens `Authentication:Twitter:ConsumerKey` e `Authentication:Twitter:ConsumerSecret`.</span><span class="sxs-lookup"><span data-stu-id="483b3-123">For the purposes of this tutorial, name the tokens `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret`.</span></span>

<span data-ttu-id="483b3-124">Esses tokens podem ser encontradas no **chaves e Tokens de acesso** guia depois de criar seu novo aplicativo Twitter:</span><span class="sxs-lookup"><span data-stu-id="483b3-124">These tokens can be found on the **Keys and Access Tokens** tab after creating your new Twitter application:</span></span>

![Guia de chaves e Tokens de acesso](index/_static/TwitterKeys.png)

## <a name="configure-twitter-authentication"></a><span data-ttu-id="483b3-126">Configurar a autenticação do Twitter</span><span class="sxs-lookup"><span data-stu-id="483b3-126">Configure Twitter Authentication</span></span>

<span data-ttu-id="483b3-127">O modelo de projeto usado neste tutorial garante que [Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter) pacote já está instalado.</span><span class="sxs-lookup"><span data-stu-id="483b3-127">The project template used in this tutorial ensures that [Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter) package is already installed.</span></span>

* <span data-ttu-id="483b3-128">Para instalar este pacote com o Visual Studio 2017, clique com botão direito no projeto e selecione **gerenciar pacotes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="483b3-128">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="483b3-129">Para instalar o .NET Core CLI, execute o seguinte comando no diretório do projeto:</span><span class="sxs-lookup"><span data-stu-id="483b3-129">To install with .NET Core CLI, execute the following in your project directory:</span></span>

   `dotnet add package Microsoft.AspNetCore.Authentication.Twitter`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="483b3-130">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="483b3-130">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="483b3-131">Adicione o serviço do Twitter no `ConfigureServices` método *Startup.cs* arquivo:</span><span class="sxs-lookup"><span data-stu-id="483b3-131">Add the Twitter service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="483b3-132">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="483b3-132">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="483b3-133">Adicionar middleware do Twitter no `Configure` método *Startup.cs* arquivo:</span><span class="sxs-lookup"><span data-stu-id="483b3-133">Add the Twitter middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseTwitterAuthentication(new TwitterOptions()
{
    ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"],
    ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"]
});
```

---

<span data-ttu-id="483b3-134">Consulte o [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) referência de API para obter mais informações sobre opções de configuração com suporte pela autenticação do Twitter.</span><span class="sxs-lookup"><span data-stu-id="483b3-134">See the [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) API reference for more information on configuration options supported by Twitter authentication.</span></span> <span data-ttu-id="483b3-135">Isso pode ser usado para solicitar informações diferentes sobre o usuário.</span><span class="sxs-lookup"><span data-stu-id="483b3-135">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-twitter"></a><span data-ttu-id="483b3-136">Entrar com o Twitter</span><span class="sxs-lookup"><span data-stu-id="483b3-136">Sign in with Twitter</span></span>

<span data-ttu-id="483b3-137">Execute o aplicativo e clique em **login**.</span><span class="sxs-lookup"><span data-stu-id="483b3-137">Run your application and click **Log in**.</span></span> <span data-ttu-id="483b3-138">Será exibida uma opção para entrar com o Twitter:</span><span class="sxs-lookup"><span data-stu-id="483b3-138">An option to sign in with Twitter appears:</span></span>

![Aplicativo Web: usuário não autenticado](index/_static/DoneTwitter.png)

<span data-ttu-id="483b3-140">Clicando em **Twitter** redireciona para o Twitter para autenticação:</span><span class="sxs-lookup"><span data-stu-id="483b3-140">Clicking on **Twitter** redirects to Twitter for authentication:</span></span>

![Página de autenticação do Twitter](index/_static/TwitterLogin.png)

<span data-ttu-id="483b3-142">Depois de inserir suas credenciais do Twitter, você será redirecionado para o site onde você pode definir seu email.</span><span class="sxs-lookup"><span data-stu-id="483b3-142">After entering your Twitter credentials, you are redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="483b3-143">Agora você está conectado usando suas credenciais do Twitter:</span><span class="sxs-lookup"><span data-stu-id="483b3-143">You are now logged in using your Twitter credentials:</span></span>

![Aplicativo Web: usuário autenticado](index/_static/Done.png)

## <a name="troubleshooting"></a><span data-ttu-id="483b3-145">Solução de problemas</span><span class="sxs-lookup"><span data-stu-id="483b3-145">Troubleshooting</span></span>

* <span data-ttu-id="483b3-146">Apenas **ASP.NET Core 2.x:** se a identidade não for configurada chamando `services.AddIdentity` no `ConfigureServices`, a autenticação resultará em *ArgumentException: a opção 'SignInScheme' deve ser fornecida*.</span><span class="sxs-lookup"><span data-stu-id="483b3-146">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="483b3-147">O modelo de projeto usado neste tutorial garante que isso é feito.</span><span class="sxs-lookup"><span data-stu-id="483b3-147">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="483b3-148">Se o banco de dados do site não tiver sido criado, aplicando a migração inicial, você obterá *uma operação de banco de dados falhou ao processar a solicitação* erro.</span><span class="sxs-lookup"><span data-stu-id="483b3-148">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="483b3-149">Toque em **aplicar migrações** para criar o banco de dados e a atualização para continuar após o erro.</span><span class="sxs-lookup"><span data-stu-id="483b3-149">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="483b3-150">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="483b3-150">Next steps</span></span>

* <span data-ttu-id="483b3-151">Este artigo mostrou como você pode autenticar com o Twitter.</span><span class="sxs-lookup"><span data-stu-id="483b3-151">This article showed how you can authenticate with Twitter.</span></span> <span data-ttu-id="483b3-152">Você pode seguir uma abordagem semelhante para autenticar com outros provedores listados no [página anterior](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="483b3-152">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="483b3-153">Depois de publicar seu site da web para o aplicativo web do Azure, você deve redefinir o `ConsumerSecret` no portal do desenvolvedor do Twitter.</span><span class="sxs-lookup"><span data-stu-id="483b3-153">Once you publish your web site to Azure web app, you should reset the `ConsumerSecret` in the Twitter developer portal.</span></span>

* <span data-ttu-id="483b3-154">Definir o `Authentication:Twitter:ConsumerKey` e `Authentication:Twitter:ConsumerSecret` como configurações de aplicativo no portal do Azure.</span><span class="sxs-lookup"><span data-stu-id="483b3-154">Set the `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="483b3-155">O sistema de configuração é configurado para ler as chaves de variáveis de ambiente.</span><span class="sxs-lookup"><span data-stu-id="483b3-155">The configuration system is set up to read keys from environment variables.</span></span>
