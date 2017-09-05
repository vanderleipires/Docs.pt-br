---
title: "Configuração de logon externo do Twitter"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 11/1/2016
ms.topic: article
ms.assetid: E5931607-31C0-4B20-B416-85E3550F5EA8
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/twitter-logins
ms.openlocfilehash: 800f98285859a54198b76411aea000384de05cd3
ms.sourcegitcommit: 74e22e08e3b08cb576e5184d16f4af5656c13c0c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/25/2017
---
# <a name="configuring-twitter-authentication"></a><span data-ttu-id="c1679-103">Configurar a autenticação do Twitter</span><span class="sxs-lookup"><span data-stu-id="c1679-103">Configuring Twitter authentication</span></span>

<a name=security-authentication-twitter-logins></a>

<span data-ttu-id="c1679-104">Por [Valeriy Novytskyy](https://github.com/01binary) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c1679-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c1679-105">Este tutorial mostra como habilitar usuários para [entrar com sua conta do Twitter](https://dev.twitter.com/web/sign-in/desktop-browser) usando um projeto do ASP.NET Core 2.0 de exemplo criado na [página anterior](index.md).</span><span class="sxs-lookup"><span data-stu-id="c1679-105">This tutorial shows you how to enable your users to [sign in with their Twitter account](https://dev.twitter.com/web/sign-in/desktop-browser) using a sample ASP.NET Core 2.0 project created on the [previous page](index.md).</span></span>

## <a name="create-the-app-in-twitter"></a><span data-ttu-id="c1679-106">Criar o aplicativo no Twitter</span><span class="sxs-lookup"><span data-stu-id="c1679-106">Create the app in Twitter</span></span>

* <span data-ttu-id="c1679-107">Navegue até [https://apps.twitter.com/](https://apps.twitter.com/) e entrar.</span><span class="sxs-lookup"><span data-stu-id="c1679-107">Navigate to [https://apps.twitter.com/](https://apps.twitter.com/) and sign in.</span></span> <span data-ttu-id="c1679-108">Se você ainda não tiver uma conta do Twitter, use o  **[inscrever-se agora](https://twitter.com/signup)**  link para criar uma.</span><span class="sxs-lookup"><span data-stu-id="c1679-108">If you don't already have a Twitter account, use the **[Sign up now](https://twitter.com/signup)** link to create one.</span></span> <span data-ttu-id="c1679-109">Depois de entrar, o **gerenciamento de aplicativos** página é mostrada:</span><span class="sxs-lookup"><span data-stu-id="c1679-109">After signing in, the **Application Management** page is shown:</span></span>

![Abra o gerenciamento de aplicativos no Microsoft Edge do Twitter](index/_static/TwitterAppManage.png)

* <span data-ttu-id="c1679-111">Toque em **criar novo aplicativo** e preencha o aplicativo **nome**, **descrição** pública e **site** URI (que pode ser temporária até que você Registre o nome de domínio):</span><span class="sxs-lookup"><span data-stu-id="c1679-111">Tap **Create New App** and fill out the application **Name**, **Description** and public **Website** URI (this can be temporary until you register the domain name):</span></span>

![Criar uma página de aplicativo](index/_static/TwitterCreate.png)

* <span data-ttu-id="c1679-113">Insira o URI de desenvolvimento com */signin-twitter* acrescentados no **válido URIs de redirecionamento OAuth** campo (por exemplo: `https://localhost:44320/signin-twitter`).</span><span class="sxs-lookup"><span data-stu-id="c1679-113">Enter your development URI with */signin-twitter* appended into the **Valid OAuth Redirect URIs** field (for example: `https://localhost:44320/signin-twitter`).</span></span> <span data-ttu-id="c1679-114">O esquema de autenticação do Twitter configurado posteriormente neste tutorial automaticamente manipulará as solicitações no */signin-twitter* rota para implementar o fluxo do OAuth.</span><span class="sxs-lookup"><span data-stu-id="c1679-114">The Twitter authentication scheme configured later in this tutorial will automatically handle requests at */signin-twitter* route to implement the OAuth flow.</span></span>

* <span data-ttu-id="c1679-115">Preencha o restante do formulário e toque em **criar seu aplicativo Twitter**.</span><span class="sxs-lookup"><span data-stu-id="c1679-115">Fill out the rest of the form and tap **Create your Twitter application**.</span></span> <span data-ttu-id="c1679-116">Novos detalhes do aplicativo são exibidos:</span><span class="sxs-lookup"><span data-stu-id="c1679-116">New application details are displayed:</span></span>

![Guia de detalhes na página de aplicativo](index/_static/TwitterAppDetails.png)

* <span data-ttu-id="c1679-118">Ao implantar o site será necessário rever o **gerenciamento de aplicativos** página e registrar um novo URI público.</span><span class="sxs-lookup"><span data-stu-id="c1679-118">When deploying the site you'll need to revisit the **Application Management** page and register a new public URI.</span></span>

## <a name="storing-twitter-consumerkey-and-consumersecret"></a><span data-ttu-id="c1679-119">Armazenar Twitter ConsumerKey e ConsumerSecret</span><span class="sxs-lookup"><span data-stu-id="c1679-119">Storing Twitter ConsumerKey and ConsumerSecret</span></span>

<span data-ttu-id="c1679-120">Vincular as configurações confidenciais, como o Twitter `Consumer Key` e `Consumer Secret` para sua configuração de aplicativo usando o [Manager segredo](../../app-secrets.md).</span><span class="sxs-lookup"><span data-stu-id="c1679-120">Link sensitive settings like Twitter `Consumer Key` and `Consumer Secret` to your application configuration using the [Secret Manager](../../app-secrets.md).</span></span> <span data-ttu-id="c1679-121">Para os fins deste tutorial, nomeie os tokens `Authentication:Twitter:ConsumerKey` e `Authentication:Twitter:ConsumerSecret`.</span><span class="sxs-lookup"><span data-stu-id="c1679-121">For the purposes of this tutorial, name the tokens `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret`.</span></span>

<span data-ttu-id="c1679-122">Esses tokens podem ser encontradas no **chaves e Tokens de acesso** guia depois de criar seu novo aplicativo Twitter:</span><span class="sxs-lookup"><span data-stu-id="c1679-122">These tokens can be found on the **Keys and Access Tokens** tab after creating your new Twitter application:</span></span>

![Guia de chaves e Tokens de acesso](index/_static/TwitterKeys.png)

## <a name="configure-twitter-authentication"></a><span data-ttu-id="c1679-124">Configurar a autenticação do Twitter</span><span class="sxs-lookup"><span data-stu-id="c1679-124">Configure Twitter Authentication</span></span>

<span data-ttu-id="c1679-125">O modelo de projeto usado neste tutorial garante que [Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter) pacote já está instalado.</span><span class="sxs-lookup"><span data-stu-id="c1679-125">The project template used in this tutorial ensures that [Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter) package is already installed.</span></span>

* <span data-ttu-id="c1679-126">Para instalar este pacote com 2017 do Visual Studio, clique com botão direito no projeto e selecione **gerenciar pacotes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="c1679-126">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="c1679-127">Para instalar o .NET Core CLI, execute o seguinte no diretório do projeto:</span><span class="sxs-lookup"><span data-stu-id="c1679-127">To install with .NET Core CLI, execute the following in your project directory:</span></span>

   `dotnet add package Microsoft.AspNetCore.Authentication.Twitter`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c1679-128">ASP.NET Core 2. x</span><span class="sxs-lookup"><span data-stu-id="c1679-128">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="c1679-129">Adicione o serviço do Twitter no `ConfigureServices` método *Startup.cs* arquivo:</span><span class="sxs-lookup"><span data-stu-id="c1679-129">Add the Twitter service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

```csharp
services.AddAuthentication().AddTwitter(twitterOptions =>
{
    twitterOptions.ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"];
    twitterOptions.ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"];
});
```

<span data-ttu-id="c1679-130">O `AddAuthentication` método só deve ser chamado uma vez ao adicionar vários provedores de autenticação.</span><span class="sxs-lookup"><span data-stu-id="c1679-130">The `AddAuthentication` method should only be called once when adding multiple authentication providers.</span></span> <span data-ttu-id="c1679-131">Chamadas subsequentes para que ele tem o potencial de substituição qualquer configurado anteriormente [AuthenticationOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.authenticationoptions) propriedades.</span><span class="sxs-lookup"><span data-stu-id="c1679-131">Subsequent calls to it have the potential of overriding any previously configured [AuthenticationOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.authenticationoptions) properties.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c1679-132">ASP.NET Core 1. x</span><span class="sxs-lookup"><span data-stu-id="c1679-132">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="c1679-133">Adicionar middleware do Twitter no `Configure` método *Startup.cs* arquivo:</span><span class="sxs-lookup"><span data-stu-id="c1679-133">Add the Twitter middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseTwitterAuthentication(new TwitterOptions()
{
    ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"],
    ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"]
});
```

---

<span data-ttu-id="c1679-134">Consulte o [TwitterOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.twitteroptions) referência de API para obter mais informações sobre opções de configuração com suporte pela autenticação do Twitter.</span><span class="sxs-lookup"><span data-stu-id="c1679-134">See the [TwitterOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.twitteroptions) API reference for more information on configuration options supported by Twitter authentication.</span></span> <span data-ttu-id="c1679-135">Isso pode ser usado para solicitar informações diferentes sobre o usuário.</span><span class="sxs-lookup"><span data-stu-id="c1679-135">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-twitter"></a><span data-ttu-id="c1679-136">Entrar com o Twitter</span><span class="sxs-lookup"><span data-stu-id="c1679-136">Sign in with Twitter</span></span>

<span data-ttu-id="c1679-137">Execute o aplicativo e clique em **login**.</span><span class="sxs-lookup"><span data-stu-id="c1679-137">Run your application and click **Log in**.</span></span> <span data-ttu-id="c1679-138">Será exibida uma opção para entrar com o Twitter:</span><span class="sxs-lookup"><span data-stu-id="c1679-138">An option to sign in with Twitter appears:</span></span>

![Aplicativo Web: usuário não autenticado](index/_static/DoneTwitter.png)

<span data-ttu-id="c1679-140">Clicando em **Twitter** redireciona para o Twitter para autenticação:</span><span class="sxs-lookup"><span data-stu-id="c1679-140">Clicking on **Twitter** redirects to Twitter for authentication:</span></span>

![Página de autenticação do Twitter](index/_static/TwitterLogin.png)

<span data-ttu-id="c1679-142">Depois de inserir suas credenciais do Twitter, você será redirecionado para o site onde você pode definir seu email.</span><span class="sxs-lookup"><span data-stu-id="c1679-142">After entering your Twitter credentials, you are redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="c1679-143">Agora você está conectado usando suas credenciais do Twitter:</span><span class="sxs-lookup"><span data-stu-id="c1679-143">You are now logged in using your Twitter credentials:</span></span>

![Aplicativo Web: usuário autenticado](index/_static/Done.png)

## <a name="troubleshooting"></a><span data-ttu-id="c1679-145">Solução de problemas</span><span class="sxs-lookup"><span data-stu-id="c1679-145">Troubleshooting</span></span>

* <span data-ttu-id="c1679-146">**ASP.NET Core 2. x somente:** identidade se não está configurada por meio da chamada `services.AddIdentity` na `ConfigureServices`, tentar autenticar resultará em *ArgumentException: A opção 'SignInScheme' deve ser fornecida*.</span><span class="sxs-lookup"><span data-stu-id="c1679-146">**ASP.NET Core 2.x only:** If Identity is not configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="c1679-147">O modelo de projeto usado neste tutorial garante que isso é feito.</span><span class="sxs-lookup"><span data-stu-id="c1679-147">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="c1679-148">Se o banco de dados do site não tiver sido criado, aplicando a migração inicial, você obterá *uma operação de banco de dados falhou ao processar a solicitação* erro.</span><span class="sxs-lookup"><span data-stu-id="c1679-148">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="c1679-149">Toque em **aplicar migrações** para criar o banco de dados e a atualização para continuar após o erro.</span><span class="sxs-lookup"><span data-stu-id="c1679-149">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c1679-150">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="c1679-150">Next steps</span></span>

* <span data-ttu-id="c1679-151">Este artigo mostrou como você pode autenticar com o Twitter.</span><span class="sxs-lookup"><span data-stu-id="c1679-151">This article showed how you can authenticate with Twitter.</span></span> <span data-ttu-id="c1679-152">Você pode seguir uma abordagem semelhante para autenticar com outros provedores listados no [página anterior](index.md).</span><span class="sxs-lookup"><span data-stu-id="c1679-152">You can follow a similar approach to authenticate with other providers listed on the [previous page](index.md).</span></span>

* <span data-ttu-id="c1679-153">Depois de publicar seu site da web para o aplicativo web do Azure, você deve redefinir o `ConsumerSecret` no portal do desenvolvedor do Twitter.</span><span class="sxs-lookup"><span data-stu-id="c1679-153">Once you publish your web site to Azure web app, you should reset the `ConsumerSecret` in the Twitter developer portal.</span></span>

* <span data-ttu-id="c1679-154">Definir o `Authentication:Twitter:ConsumerKey` e `Authentication:Twitter:ConsumerSecret` como configurações de aplicativo no portal do Azure.</span><span class="sxs-lookup"><span data-stu-id="c1679-154">Set the `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="c1679-155">O sistema de configuração é configurado para ler as chaves de variáveis de ambiente.</span><span class="sxs-lookup"><span data-stu-id="c1679-155">The configuration system is set up to read keys from environment variables.</span></span>
