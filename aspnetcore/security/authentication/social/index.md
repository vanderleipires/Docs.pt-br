---
title: "Autenticação de Facebook, Google e de provedor externo no ASP.NET Core"
author: rick-anderson
description: "Este tutorial demonstra como criar um aplicativo do ASP.NET Core 2.x usando o OAuth 2.0 com provedores de autenticação externa."
manager: wpickett
ms.author: riande
ms.date: 11/01/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/social/index
ms.openlocfilehash: 76433f814d6850a449434c29eb0bd27570ce193a
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/02/2018
---
# <a name="facebook-google-and-external-provider-authentication-in-aspnet-core"></a><span data-ttu-id="2882f-103">Autenticação de Facebook, Google e de provedor externo no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2882f-103">Facebook, Google, and external provider authentication in ASP.NET Core</span></span>

<a name="security-authentication-social-logins"></a>

<span data-ttu-id="2882f-104">Por [Valeriy Novytskyy](https://github.com/01binary) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="2882f-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="2882f-105">Este tutorial demonstra como criar um aplicativo ASP.NET Core 2.x que permite aos usuários fazer logon usando o OAuth 2.0 com as credenciais de provedores de autenticação externa.</span><span class="sxs-lookup"><span data-stu-id="2882f-105">This tutorial demonstrates how to build an ASP.NET Core 2.x app that enables users to log in using OAuth 2.0 with credentials from external authentication providers.</span></span>

<span data-ttu-id="2882f-106">Os provedores [Facebook](facebook-logins.md), [Twitter](twitter-logins.md), [Google](google-logins.md) e [Microsoft](microsoft-logins.md) são abordados nas seções a seguir.</span><span class="sxs-lookup"><span data-stu-id="2882f-106">[Facebook](facebook-logins.md), [Twitter](twitter-logins.md), [Google](google-logins.md), and [Microsoft](microsoft-logins.md) providers are covered in the following sections.</span></span> <span data-ttu-id="2882f-107">Outros provedores estão disponíveis em pacotes de terceiros, como [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) e [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).</span><span class="sxs-lookup"><span data-stu-id="2882f-107">Other providers are available in third-party packages such as [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) and [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).</span></span>

![Ícones de mídia social do Facebook, Twitter, Google+ e Windows](index/_static/social.png)

<span data-ttu-id="2882f-109">Permitir que os usuários entrem com suas credenciais existentes é conveniente para os usuários e transfere muitas das complexidades de gerenciar o processo de entrada para um terceiro.</span><span class="sxs-lookup"><span data-stu-id="2882f-109">Enabling users to sign in with their existing credentials is convenient for the users and shifts many of the complexities of managing the sign-in process onto a third party.</span></span> <span data-ttu-id="2882f-110">Para obter exemplos de como os logons sociais podem impulsionar o tráfego e as conversões de clientes, consulte os estudos de caso do [Facebook](https://www.facebook.com/unsupportedbrowser) e do [Twitter](https://dev.twitter.com/resources/case-studies).</span><span class="sxs-lookup"><span data-stu-id="2882f-110">For examples of how social logins can drive traffic and customer conversions, see case studies by [Facebook](https://www.facebook.com/unsupportedbrowser) and [Twitter](https://dev.twitter.com/resources/case-studies).</span></span>

<span data-ttu-id="2882f-111">Observação: os pacotes apresentados aqui eliminam uma grande parte da complexidade do fluxo de autenticação OAuth, mas o entendimento dos detalhes pode ser necessário durante a solução de problemas.</span><span class="sxs-lookup"><span data-stu-id="2882f-111">Note: Packages presented here abstract a great deal of complexity of the OAuth authentication flow, but understanding the details may become necessary when troubleshooting.</span></span> <span data-ttu-id="2882f-112">Muitos recursos estão disponíveis; por exemplo, consulte [Introdução ao OAuth 2](https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2) ou [Noções básicas do OAuth 2](http://www.bubblecode.net/2016/01/22/understanding-oauth2/).</span><span class="sxs-lookup"><span data-stu-id="2882f-112">Many resources are available; for example, see [Introduction to OAuth 2](https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2) or [Understanding OAuth 2](http://www.bubblecode.net/2016/01/22/understanding-oauth2/).</span></span> <span data-ttu-id="2882f-113">Alguns problemas podem ser resolvidos examinando o [código-fonte do ASP.NET Core para os pacotes de provedores](https://github.com/aspnet/Security/tree/dev/src).</span><span class="sxs-lookup"><span data-stu-id="2882f-113">Some issues can be resolved by looking at the [ASP.NET Core source code for the provider packages](https://github.com/aspnet/Security/tree/dev/src).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="2882f-114">Criar um novo projeto ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2882f-114">Create a New ASP.NET Core Project</span></span>

* <span data-ttu-id="2882f-115">No Visual Studio 2017, crie um novo projeto na Página Inicial ou por meio de **Arquivo > Novo > Projeto**.</span><span class="sxs-lookup"><span data-stu-id="2882f-115">In Visual Studio 2017, create a new project from the Start Page, or via **File > New > Project**.</span></span>

* <span data-ttu-id="2882f-116">Selecione o modelo **Aplicativo Web ASP.NET Core** disponível na categoria **Visual C# > .NET Core**:</span><span class="sxs-lookup"><span data-stu-id="2882f-116">Select the **ASP.NET Core Web Application** template available in **Visual C# > .NET Core** category:</span></span>

![Caixa de diálogo Novo Projeto](index/_static/new-project.png)

* <span data-ttu-id="2882f-118">Toque em **Aplicativo Web** e verifique se a opção **Autenticação** está definida como **Contas de Usuário Individuais**:</span><span class="sxs-lookup"><span data-stu-id="2882f-118">Tap **Web Application** and verify **Authentication** is set to **Individual User Accounts**:</span></span>

![Caixa de diálogo Novo Aplicativo Web](index/_static/select-project.png)

<span data-ttu-id="2882f-120">Observação: este tutorial aplica-se à versão do SDK do ASP.NET Core 2.0 que pode ser selecionada na parte superior do assistente.</span><span class="sxs-lookup"><span data-stu-id="2882f-120">Note: This tutorial applies to ASP.NET Core 2.0 SDK version which can be selected at the top of the wizard.</span></span>

## <a name="apply-migrations"></a><span data-ttu-id="2882f-121">Aplicar migrações</span><span class="sxs-lookup"><span data-stu-id="2882f-121">Apply migrations</span></span>

* <span data-ttu-id="2882f-122">Execute o aplicativo e selecione o link **login**.</span><span class="sxs-lookup"><span data-stu-id="2882f-122">Run the app and select the **Log in** link.</span></span>
* <span data-ttu-id="2882f-123">Selecione o link **Registrar como um novo usuário**.</span><span class="sxs-lookup"><span data-stu-id="2882f-123">Select the **Register as a new user** link.</span></span>
* <span data-ttu-id="2882f-124">Insira o email e a senha para a nova conta e, em seguida, selecione **Registrar**.</span><span class="sxs-lookup"><span data-stu-id="2882f-124">Enter the email and password for the new account, and then select **Register**.</span></span>
* <span data-ttu-id="2882f-125">Siga as instruções para aplicar as migrações.</span><span class="sxs-lookup"><span data-stu-id="2882f-125">Follow the instructions to apply migrations.</span></span>

## <a name="require-ssl"></a><span data-ttu-id="2882f-126">Exigir SSL</span><span class="sxs-lookup"><span data-stu-id="2882f-126">Require SSL</span></span>

<span data-ttu-id="2882f-127">O OAuth 2.0 exige o uso de SSL para autenticação por meio do protocolo HTTPS.</span><span class="sxs-lookup"><span data-stu-id="2882f-127">OAuth 2.0 requires the use of SSL for authentication over the HTTPS protocol.</span></span>

<span data-ttu-id="2882f-128">Observação: os projetos criados com o uso de modelos de projeto **Aplicativo Web** ou **API Web** para o ASP.NET Core 2.x são automaticamente configurados para habilitar o SSL e iniciados com a URL HTTPS se a opção **Contas de Usuário Individuais** foi selecionada na **caixa de diálogo Alterar Autenticação** no assistente do projeto, conforme mostrado acima.</span><span class="sxs-lookup"><span data-stu-id="2882f-128">Note: Projects created using **Web Application** or **Web API** project templates for ASP.NET Core 2.x are automatically configured to enable SSL and launch with https URL if the **Individual User Accounts** option was selected on **Change Authentication dialog** in the project wizard as shown above.</span></span>

* <span data-ttu-id="2882f-129">Exija o SSL em seu site seguindo as etapas do tópico [Impondo o SSL em um aplicativo ASP.NET Core](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="2882f-129">Require SSL on your site by following the steps in [Enforcing SSL in an ASP.NET Core app](xref:security/enforcing-ssl) topic.</span></span>

## <a name="use-secretmanager-to-store-tokens-assigned-by-login-providers"></a><span data-ttu-id="2882f-130">Usar o SecretManager para armazenar os tokens atribuídos por provedores de logon</span><span class="sxs-lookup"><span data-stu-id="2882f-130">Use SecretManager to store tokens assigned by login providers</span></span>

<span data-ttu-id="2882f-131">Os provedores de logon social atribuem tokens de **ID do Aplicativo** e **Segredo do Aplicativo** durante o processo de registro (a nomenclatura exata varia de acordo com o provedor).</span><span class="sxs-lookup"><span data-stu-id="2882f-131">Social login providers assign **Application Id** and **Application Secret** tokens during the registration process (exact naming varies by provider).</span></span>

<span data-ttu-id="2882f-132">Esses valores são efetivamente o *nome de usuário* e a *senha* que o aplicativo usa para acessar sua API e constituem os “segredos” que podem ser vinculados à configuração do aplicativo com a ajuda do **Secret Manager**, em vez de armazená-los em arquivos de configuração diretamente ou embuti-los em código.</span><span class="sxs-lookup"><span data-stu-id="2882f-132">These values are effectively the *user name* and *password* your application uses to access their API, and constitute the "secrets" that can be linked to your application configuration with the help of **Secret Manager** instead of storing them in configuration files directly or hard-coding them.</span></span>

<span data-ttu-id="2882f-133">Siga as etapas do tópico [Armazenamento seguro de segredos do aplicativo durante o desenvolvimento no ASP.NET Core](xref:security/app-secrets) para que você possa armazenar os tokens atribuídos por cada provedor de logon abaixo.</span><span class="sxs-lookup"><span data-stu-id="2882f-133">Follow the steps in [Safe storage of app secrets during development in ASP.NET Core](xref:security/app-secrets) topic so that you can store tokens assigned by each login provider below.</span></span>

## <a name="setup-login-providers-required-by-your-application"></a><span data-ttu-id="2882f-134">Configurar os provedores de logon necessários para o aplicativo</span><span class="sxs-lookup"><span data-stu-id="2882f-134">Setup login providers required by your application</span></span>

<span data-ttu-id="2882f-135">Use os seguintes tópicos para configurar seu aplicativo para usar os respectivos provedores:</span><span class="sxs-lookup"><span data-stu-id="2882f-135">Use the following topics to configure your application to use the respective providers:</span></span>

* <span data-ttu-id="2882f-136">Instruções do [Facebook](facebook-logins.md)</span><span class="sxs-lookup"><span data-stu-id="2882f-136">[Facebook](facebook-logins.md) instructions</span></span>
* <span data-ttu-id="2882f-137">Instruções do [Twitter](twitter-logins.md)</span><span class="sxs-lookup"><span data-stu-id="2882f-137">[Twitter](twitter-logins.md) instructions</span></span>
* <span data-ttu-id="2882f-138">Instruções do [Google](google-logins.md)</span><span class="sxs-lookup"><span data-stu-id="2882f-138">[Google](google-logins.md) instructions</span></span>
* <span data-ttu-id="2882f-139">Instruções da [Microsoft](microsoft-logins.md)</span><span class="sxs-lookup"><span data-stu-id="2882f-139">[Microsoft](microsoft-logins.md) instructions</span></span>
* <span data-ttu-id="2882f-140">[Outras](other-logins.md) instruções do provedor</span><span class="sxs-lookup"><span data-stu-id="2882f-140">[Other provider](other-logins.md) instructions</span></span>

## <a name="optionally-set-password"></a><span data-ttu-id="2882f-141">Definir a senha opcionalmente</span><span class="sxs-lookup"><span data-stu-id="2882f-141">Optionally set password</span></span>

<span data-ttu-id="2882f-142">Ao registrar um provedor de logon externo, você não precisa ter uma senha registrada no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="2882f-142">When you register with an external login provider, you don't have a password registered with the app.</span></span> <span data-ttu-id="2882f-143">Isso o alivia da tarefa de criar e lembrar de uma senha para o site, mas também o torna dependente do provedor de logon externo.</span><span class="sxs-lookup"><span data-stu-id="2882f-143">This alleviates you from creating and remembering a password for the site, but it also makes you dependent on the external login provider.</span></span> <span data-ttu-id="2882f-144">Se o provedor de logon externo não estiver disponível, você não poderá fazer logon no site.</span><span class="sxs-lookup"><span data-stu-id="2882f-144">If the external login provider is unavailable, you won't be able to log in to the web site.</span></span>

<span data-ttu-id="2882f-145">Para criar uma senha e entrar usando seu email definido durante o processo de entrada com provedores externos:</span><span class="sxs-lookup"><span data-stu-id="2882f-145">To create a password and sign in using your email that you set during the sign in process with external providers:</span></span>

* <span data-ttu-id="2882f-146">Toque no link **Olá <email alias>** na parte superior direita para navegar para a exibição **Gerenciar**.</span><span class="sxs-lookup"><span data-stu-id="2882f-146">Tap the **Hello <email alias>** link at the top right corner to navigate to the **Manage** view.</span></span>

![Exibição Gerenciar do Aplicativo Web](index/_static/pass1a.png)

* <span data-ttu-id="2882f-148">Toque em **Criar**</span><span class="sxs-lookup"><span data-stu-id="2882f-148">Tap **Create**</span></span>

![Definir a página de senha](index/_static/pass2a.png)

* <span data-ttu-id="2882f-150">Defina uma senha válida e use-a para entrar com seu email.</span><span class="sxs-lookup"><span data-stu-id="2882f-150">Set a valid password and you can use this to sign in with your email.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2882f-151">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="2882f-151">Next steps</span></span>

* <span data-ttu-id="2882f-152">Este artigo apresentou a autenticação externa e explicou os pré-requisitos necessários para adicionar logons externos ao aplicativo ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2882f-152">This article introduced external authentication and explained the prerequisites required to add external logins to your ASP.NET Core app.</span></span>

* <span data-ttu-id="2882f-153">Páginas de referência específicas ao provedor para configurar logons para os provedores necessários para o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="2882f-153">Reference provider-specific pages to configure logins for the providers required by your app.</span></span>
