---
title: "Habilitando a autenticação usando o Facebook, o Google e outros provedores externos"
author: rick-anderson
description: 
keywords: "ASP.NET Core, autenticação, social, provedores de autenticação, Google, Facebook, Twitter, conta da Microsoft"
ms.author: riande
manager: wpickett
ms.date: 11/01/2016
ms.topic: article
ms.assetid: eda7ee17-f38c-462e-8d1d-63f459901cf3
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/social/index
ms.openlocfilehash: b561dcee5435dfc34cfa0b9b15babf75ca8f3508
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/22/2017
---
# <a name="enabling-authentication-using-facebook-google-and-other-external-providers"></a><span data-ttu-id="30469-103">Habilitando a autenticação usando o Facebook, o Google e outros provedores externos</span><span class="sxs-lookup"><span data-stu-id="30469-103">Enabling authentication using Facebook, Google and other external providers</span></span>

<a name=security-authentication-social-logins></a>

<span data-ttu-id="30469-104">Por [Valeriy Novytskyy](https://github.com/01binary) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="30469-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="30469-105">Este tutorial demonstra como criar um aplicativo ASP.NET Core 2.x que permite aos usuários fazer logon usando o OAuth 2.0 com as credenciais de provedores de autenticação externa.</span><span class="sxs-lookup"><span data-stu-id="30469-105">This tutorial demonstrates how to build an ASP.NET Core 2.x app that enables users to log in using OAuth 2.0 with credentials from external authentication providers.</span></span>

<span data-ttu-id="30469-106">Os provedores [Facebook](facebook-logins.md), [Twitter](twitter-logins.md), [Google](google-logins.md) e [Microsoft](microsoft-logins.md) são abordados nas seções a seguir.</span><span class="sxs-lookup"><span data-stu-id="30469-106">[Facebook](facebook-logins.md), [Twitter](twitter-logins.md), [Google](google-logins.md), and [Microsoft](microsoft-logins.md) providers are covered in the following sections.</span></span> <span data-ttu-id="30469-107">Outros provedores estão disponíveis em pacotes de terceiros, como [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) e [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).</span><span class="sxs-lookup"><span data-stu-id="30469-107">Other providers are available in third-party packages such as [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) and [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).</span></span>

![Ícones de mídia social do Facebook, Twitter, Google+ e Windows](index/_static/social.png)

<span data-ttu-id="30469-109">Permitir que os usuários entrem com suas credenciais existentes é conveniente para os usuários e transfere muitas das complexidades de gerenciar o processo de entrada para um terceiro.</span><span class="sxs-lookup"><span data-stu-id="30469-109">Enabling users to sign in with their existing credentials is convenient for the users and shifts many of the complexities of managing the sign-in process onto a third party.</span></span> <span data-ttu-id="30469-110">Para obter exemplos de como os logons sociais podem impulsionar o tráfego e as conversões de clientes, consulte os estudos de caso do [Facebook](https://www.facebook.com/unsupportedbrowser) e do [Twitter](https://dev.twitter.com/resources/case-studies).</span><span class="sxs-lookup"><span data-stu-id="30469-110">For examples of how social logins can drive traffic and customer conversions, see case studies by [Facebook](https://www.facebook.com/unsupportedbrowser) and [Twitter](https://dev.twitter.com/resources/case-studies).</span></span>

<span data-ttu-id="30469-111">Observação: os pacotes apresentados aqui eliminam uma grande parte da complexidade do fluxo de autenticação OAuth, mas o entendimento dos detalhes pode ser necessário durante a solução de problemas.</span><span class="sxs-lookup"><span data-stu-id="30469-111">Note: Packages presented here abstract a great deal of complexity of the OAuth authentication flow, but understanding the details may become necessary when troubleshooting.</span></span> <span data-ttu-id="30469-112">Muitos recursos estão disponíveis; por exemplo, consulte [Introdução ao OAuth 2](https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2) ou [Noções básicas do OAuth 2](http://www.bubblecode.net/2016/01/22/understanding-oauth2/).</span><span class="sxs-lookup"><span data-stu-id="30469-112">Many resources are available; for example, see [Introduction to OAuth 2](https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2) or [Understanding OAuth 2](http://www.bubblecode.net/2016/01/22/understanding-oauth2/).</span></span> <span data-ttu-id="30469-113">Alguns problemas podem ser resolvidos examinando o [código-fonte do ASP.NET Core para os pacotes de provedores](https://github.com/aspnet/Security/tree/dev/src).</span><span class="sxs-lookup"><span data-stu-id="30469-113">Some issues can be resolved by looking at the [ASP.NET Core source code for the provider packages](https://github.com/aspnet/Security/tree/dev/src).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="30469-114">Criar um novo projeto ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="30469-114">Create a New ASP.NET Core Project</span></span>

* <span data-ttu-id="30469-115">No Visual Studio 2017, crie um novo projeto na Página Inicial ou por meio de **Arquivo > Novo > Projeto**.</span><span class="sxs-lookup"><span data-stu-id="30469-115">In Visual Studio 2017, create a new project from the Start Page, or via **File > New > Project**.</span></span>

* <span data-ttu-id="30469-116">Selecione o modelo **Aplicativo Web ASP.NET Core** disponível na categoria **Visual C# > .NET Core**:</span><span class="sxs-lookup"><span data-stu-id="30469-116">Select the **ASP.NET Core Web Application** template available in **Visual C# > .NET Core** category:</span></span>

![Caixa de diálogo Novo Projeto](index/_static/new-project.png)

* <span data-ttu-id="30469-118">Toque em **Aplicativo Web** e verifique se a opção **Autenticação** está definida como **Contas de Usuário Individuais**:</span><span class="sxs-lookup"><span data-stu-id="30469-118">Tap **Web Application** and verify **Authentication** is set to **Individual User Accounts**:</span></span>

![Caixa de diálogo Novo Aplicativo Web](index/_static/select-project.png)

<span data-ttu-id="30469-120">Observação: este tutorial aplica-se à versão do SDK do ASP.NET Core 2.0 que pode ser selecionada na parte superior do assistente.</span><span class="sxs-lookup"><span data-stu-id="30469-120">Note: This tutorial applies to ASP.NET Core 2.0 SDK version which can be selected at the top of the wizard.</span></span>

## <a name="require-ssl"></a><span data-ttu-id="30469-121">Exigir SSL</span><span class="sxs-lookup"><span data-stu-id="30469-121">Require SSL</span></span>

<span data-ttu-id="30469-122">O OAuth 2.0 exige o uso de SSL para autenticação por meio do protocolo HTTPS.</span><span class="sxs-lookup"><span data-stu-id="30469-122">OAuth 2.0 requires the use of SSL for authentication over the HTTPS protocol.</span></span>

<span data-ttu-id="30469-123">Observação: os projetos criados com o uso de modelos de projeto **Aplicativo Web** ou **API Web** para o ASP.NET Core 2.x são automaticamente configurados para habilitar o SSL e iniciados com a URL HTTPS se a opção **Contas de Usuário Individuais** foi selecionada na **caixa de diálogo Alterar Autenticação** no assistente do projeto, conforme mostrado acima.</span><span class="sxs-lookup"><span data-stu-id="30469-123">Note: Projects created using **Web Application** or **Web API** project templates for ASP.NET Core 2.x are automatically configured to enable SSL and launch with https URL if the **Individual User Accounts** option was selected on **Change Authentication dialog** in the project wizard as shown above.</span></span>

* <span data-ttu-id="30469-124">Saiba como habilitar o SSL manualmente seguindo as etapas do tópico [Configurando o HTTPS para desenvolvimento no ASP.NET Core](xref:security/https).</span><span class="sxs-lookup"><span data-stu-id="30469-124">Learn how to enable SSL manually by following the steps in [Setting up HTTPS for development in ASP.NET Core](xref:security/https) topic.</span></span>

* <span data-ttu-id="30469-125">Em seguida, exija o SSL em seu site seguindo as etapas do tópico [Impondo o SSL em um aplicativo ASP.NET Core](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="30469-125">Then, require SSL on your site by following the steps in [Enforcing SSL in an ASP.NET Core app](xref:security/enforcing-ssl) topic.</span></span>

## <a name="use-secretmanager-to-store-tokens-assigned-by-login-providers"></a><span data-ttu-id="30469-126">Usar o SecretManager para armazenar os tokens atribuídos por provedores de logon</span><span class="sxs-lookup"><span data-stu-id="30469-126">Use SecretManager to store tokens assigned by login providers</span></span>

<span data-ttu-id="30469-127">Os provedores de logon social atribuem tokens de **ID do Aplicativo** e **Segredo do Aplicativo** durante o processo de registro (a nomenclatura exata varia de acordo com o provedor).</span><span class="sxs-lookup"><span data-stu-id="30469-127">Social login providers assign **Application Id** and **Application Secret** tokens during the registration process (exact naming varies by provider).</span></span>

<span data-ttu-id="30469-128">Esses valores são efetivamente o *nome de usuário* e a *senha* que o aplicativo usa para acessar sua API e constituem os “segredos” que podem ser vinculados à configuração do aplicativo com a ajuda do **Secret Manager**, em vez de armazená-los em arquivos de configuração diretamente ou embuti-los em código.</span><span class="sxs-lookup"><span data-stu-id="30469-128">These values are effectively the *user name* and *password* your application uses to access their API, and constitute the "secrets" that can be linked to your application configuration with the help of **Secret Manager** instead of storing them in configuration files directly or hard-coding them.</span></span>

<span data-ttu-id="30469-129">Siga as etapas do tópico [Armazenamento seguro de segredos do aplicativo durante o desenvolvimento no ASP.NET Core](xref:security/app-secrets) para que você possa armazenar os tokens atribuídos por cada provedor de logon abaixo.</span><span class="sxs-lookup"><span data-stu-id="30469-129">Follow the steps in [Safe storage of app secrets during development in ASP.NET Core](xref:security/app-secrets) topic so that you can store tokens assigned by each login provider below.</span></span>

## <a name="setup-login-providers-required-by-your-application"></a><span data-ttu-id="30469-130">Configurar os provedores de logon necessários para o aplicativo</span><span class="sxs-lookup"><span data-stu-id="30469-130">Setup login providers required by your application</span></span>

<span data-ttu-id="30469-131">Use os seguintes tópicos para configurar seu aplicativo para usar os respectivos provedores:</span><span class="sxs-lookup"><span data-stu-id="30469-131">Use the following topics to configure your application to use the respective providers:</span></span>

* <span data-ttu-id="30469-132">Instruções do [Facebook](facebook-logins.md)</span><span class="sxs-lookup"><span data-stu-id="30469-132">[Facebook](facebook-logins.md) instructions</span></span>
* <span data-ttu-id="30469-133">Instruções do [Twitter](twitter-logins.md)</span><span class="sxs-lookup"><span data-stu-id="30469-133">[Twitter](twitter-logins.md) instructions</span></span>
* <span data-ttu-id="30469-134">Instruções do [Google](google-logins.md)</span><span class="sxs-lookup"><span data-stu-id="30469-134">[Google](google-logins.md) instructions</span></span>
* <span data-ttu-id="30469-135">Instruções da [Microsoft](microsoft-logins.md)</span><span class="sxs-lookup"><span data-stu-id="30469-135">[Microsoft](microsoft-logins.md) instructions</span></span>
* <span data-ttu-id="30469-136">[Outras](other-logins.md) instruções do provedor</span><span class="sxs-lookup"><span data-stu-id="30469-136">[Other provider](other-logins.md) instructions</span></span>

## <a name="optionally-set-password"></a><span data-ttu-id="30469-137">Definir a senha opcionalmente</span><span class="sxs-lookup"><span data-stu-id="30469-137">Optionally set password</span></span>

<span data-ttu-id="30469-138">Ao registrar um provedor de logon externo, você não precisa ter uma senha registrada no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="30469-138">When you register with an external login provider, you do not have a password registered with the app.</span></span> <span data-ttu-id="30469-139">Isso o alivia da tarefa de criar e lembrar de uma senha para o site, mas também o torna dependente do provedor de logon externo.</span><span class="sxs-lookup"><span data-stu-id="30469-139">This alleviates you from creating and remembering a password for the site, but it also makes you dependent on the external login provider.</span></span> <span data-ttu-id="30469-140">Se o provedor de logon externo não estiver disponível, você não poderá fazer logon no site.</span><span class="sxs-lookup"><span data-stu-id="30469-140">If the external login provider is unavailable, you won't be able to log in to the web site.</span></span>

<span data-ttu-id="30469-141">Para criar uma senha e entrar usando seu email definido durante o processo de entrada com provedores externos:</span><span class="sxs-lookup"><span data-stu-id="30469-141">To create a password and sign in using your email that you set during the sign in process with external providers:</span></span>

* <span data-ttu-id="30469-142">Toque no link **Olá <email alias>** na parte superior direita para navegar para a exibição **Gerenciar**.</span><span class="sxs-lookup"><span data-stu-id="30469-142">Tap the **Hello <email alias>** link at the top right corner to navigate to the **Manage** view.</span></span>

![Exibição Gerenciar do Aplicativo Web](index/_static/pass1a.png)

* <span data-ttu-id="30469-144">Toque em **Criar**</span><span class="sxs-lookup"><span data-stu-id="30469-144">Tap **Create**</span></span>

![Definir a página de senha](index/_static/pass2a.png)

* <span data-ttu-id="30469-146">Defina uma senha válida e use-a para entrar com seu email.</span><span class="sxs-lookup"><span data-stu-id="30469-146">Set a valid password and you can use this to sign in with your email.</span></span>

## <a name="next-steps"></a><span data-ttu-id="30469-147">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="30469-147">Next steps</span></span>

* <span data-ttu-id="30469-148">Este artigo apresentou a autenticação externa e explicou os pré-requisitos necessários para adicionar logons externos ao aplicativo ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="30469-148">This article introduced external authentication and explained the prerequisites required to add external logins to your ASP.NET Core app.</span></span>

* <span data-ttu-id="30469-149">Páginas de referência específicas ao provedor para configurar logons para os provedores necessários para o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="30469-149">Reference provider-specific pages to configure logins for the providers required by your app.</span></span>
