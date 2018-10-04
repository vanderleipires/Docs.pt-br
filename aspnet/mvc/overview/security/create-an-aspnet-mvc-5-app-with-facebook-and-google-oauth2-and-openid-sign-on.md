---
uid: mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
title: Criar o MVC 5 aplicativo com o Facebook, Twitter, LinkedIn e Google OAuth2 Sign-on (c#) | Microsoft Docs
author: Rick-Anderson
description: Este tutorial mostra como criar um aplicativo web ASP.NET MVC 5 que permite que os usuários façam logon usando o OAuth 2.0 com as credenciais de um autenti externo...
ms.author: riande
ms.date: 04/03/2015
ms.assetid: 81ee500f-fc37-40d6-8722-f1b64720fbb6
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
msc.type: authoredcontent
ms.openlocfilehash: 611a4b59b2ea2eee771f4060fb5d5af041b2ccc6
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577763"
---
<a name="create-an-aspnet-mvc-5-app-with-facebook-twitter-linkedin-and-google-oauth2-sign-on-c"></a><span data-ttu-id="e2ffd-103">Criar um aplicativo ASP.NET MVC 5 com o Facebook, Twitter, LinkedIn e Google OAuth2 Sign-on (c#)</span><span class="sxs-lookup"><span data-stu-id="e2ffd-103">Create an ASP.NET MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on (C#)</span></span>
====================
<span data-ttu-id="e2ffd-104">por [Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="e2ffd-104">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

> <span data-ttu-id="e2ffd-105">Este tutorial mostra como criar um aplicativo web ASP.NET MVC 5 que permite que os usuários façam logon usando [OAuth 2.0](http://oauth.net/2/) com as credenciais de um provedor de autenticação externa, como o Facebook, Twitter, LinkedIn, Microsoft ou Google.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-105">This tutorial shows you how to build an ASP.NET MVC 5 web application that enables users to log in using [OAuth 2.0](http://oauth.net/2/) with credentials from an external authentication provider, such as Facebook, Twitter, LinkedIn, Microsoft, or Google.</span></span> <span data-ttu-id="e2ffd-106">Para simplificar, este tutorial concentra-se sobre como trabalhar com as credenciais do Facebook e Google.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-106">For simplicity, this tutorial focuses on working with credentials from Facebook and Google.</span></span>
> 
> <span data-ttu-id="e2ffd-107">Habilitar essas credenciais em sites da web fornece uma vantagem significativa porque milhões de usuários já têm contas com esses provedores externos.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-107">Enabling these credentials in your web sites provides a significant advantage because millions of users already have accounts with these external providers.</span></span> <span data-ttu-id="e2ffd-108">Esses usuários podem ser mais decididos a se inscrever para seu site se eles não precisam criar e lembrar de um novo conjunto de credenciais.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-108">These users may be more inclined to sign up for your site if they do not have to create and remember a new set of credentials.</span></span>
> 
> <span data-ttu-id="e2ffd-109">Consulte também [aplicativo ASP.NET MVC 5 com o SMS e email de autenticação de dois fatores](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="e2ffd-109">See also [ASP.NET MVC 5 app with SMS and email Two-Factor Authentication](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).</span></span>
> 
> <span data-ttu-id="e2ffd-110">O tutorial também mostra como adicionar dados de perfil do usuário e como usar a API de associação para adicionar funções.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-110">The tutorial also shows how to add profile data for the user, and how to use the Membership API to add roles.</span></span> <span data-ttu-id="e2ffd-111">Este tutorial foi escrito por [Rick Anderson](https://blogs.msdn.com/rickAndy) (siga-me no Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).</span><span class="sxs-lookup"><span data-stu-id="e2ffd-111">This tutorial was written by [Rick Anderson](https://blogs.msdn.com/rickAndy) ( Please follow me on Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).</span></span>


<a id="start"></a>
## <a name="getting-started"></a><span data-ttu-id="e2ffd-112">Guia de Introdução</span><span class="sxs-lookup"><span data-stu-id="e2ffd-112">Getting Started</span></span>

<span data-ttu-id="e2ffd-113">Comece instalando e executando [Visual Studio Express 2013 para Web](https://go.microsoft.com/fwlink/?LinkId=299058) ou [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="e2ffd-113">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="e2ffd-114">Instalar o Visual Studio [2013 atualização 3](https://go.microsoft.com/fwlink/?LinkId=390521) ou superior.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-114">Install Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) or higher.</span></span> <span data-ttu-id="e2ffd-115">Para obter ajuda com o Dropbox, GitHub, Linkedin, Instagram, Buffer, Salesforce, STEAM, Stack Exchange, Tripit, Twitch, Twitter, Yahoo! e muito mais, consulte este [projeto de exemplo](https://github.com/matthewdunsdon/oauthforaspnet).</span><span class="sxs-lookup"><span data-stu-id="e2ffd-115">For help with Dropbox, GitHub, Linkedin, Instagram, Buffer, Salesforce, STEAM, Stack Exchange, Tripit, Twitch, Twitter, Yahoo!, and more, see this [sample project](https://github.com/matthewdunsdon/oauthforaspnet).</span></span>

> [!NOTE]
> <span data-ttu-id="e2ffd-116">Você deve instalar o Visual Studio [2013 atualização 3](https://go.microsoft.com/fwlink/?LinkId=390521) ou superior para usar o Google OAuth 2 e depurar localmente sem avisos de SSL.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-116">You must install Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) or higher to use Google OAuth 2 and to debug locally without SSL warnings.</span></span>


<span data-ttu-id="e2ffd-117">Clique em **novo projeto** da **inicie** página, ou você pode usar o menu e selecione **arquivo**e, em seguida, **novo projeto**.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-117">Click **New Project** from the **Start** page, or you can use the menu and select **File**, and then **New Project**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image1.png)  
 

<a id="1st"></a>
## <a name="creating-your-first-application"></a><span data-ttu-id="e2ffd-118">Criando seu primeiro aplicativo</span><span class="sxs-lookup"><span data-stu-id="e2ffd-118">Creating Your First Application</span></span>

<span data-ttu-id="e2ffd-119">Clique em **novo projeto**, em seguida, selecione **Visual c#** à esquerda, em seguida, **Web** e, em seguida, selecione **aplicativo Web ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-119">Click **New Project**, then select **Visual C#** on the left, then **Web** and then select **ASP.NET Web Application**.</span></span> <span data-ttu-id="e2ffd-120">Nomeie o projeto "MvcAuth" e, em seguida, clique em **Okey**.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-120">Name your project "MvcAuth" and then click **OK**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image2.png)

<span data-ttu-id="e2ffd-121">No **novo projeto ASP.NET** caixa de diálogo, clique em **MVC**.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-121">In the **New ASP.NET Project** dialog, click **MVC**.</span></span> <span data-ttu-id="e2ffd-122">Se a autenticação não estiver **contas de usuário individuais**, clique no **alterar autenticação** botão e selecione **contas de usuário individuais**.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-122">If the Authentication is not **Individual User Accounts**, click the **Change Authentication** button and select **Individual User Accounts**.</span></span> <span data-ttu-id="e2ffd-123">Verificando **hospedar na nuvem**, o aplicativo será muito fácil hospedar no Azure.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-123">By checking **Host in the cloud**, the app will be very easy to host in Azure.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image3.png)

<span data-ttu-id="e2ffd-124">Se você selecionou **hospedar na nuvem**, conclua a caixa de diálogo Configurar.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-124">If you selected **Host in the cloud**, complete the configure dialog.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image4.png)


### <a name="use-nuget-to-update-to-the-latest-owin-middleware"></a><span data-ttu-id="e2ffd-125">Use o NuGet para atualizar para o middleware OWIN mais recente</span><span class="sxs-lookup"><span data-stu-id="e2ffd-125">Use NuGet to update to the latest OWIN middleware</span></span>

<span data-ttu-id="e2ffd-126">Use o Gerenciador de pacotes do NuGet para atualizar o [middleware OWIN](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md).</span><span class="sxs-lookup"><span data-stu-id="e2ffd-126">Use the NuGet package manager to update the [OWIN middleware](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md).</span></span> <span data-ttu-id="e2ffd-127">Selecione **atualizações** no menu à esquerda.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-127">Select **Updates** in the left menu.</span></span> <span data-ttu-id="e2ffd-128">Você pode clicar na **Atualizar tudo** botão ou você pode procurar apenas os pacotes OWIN (mostrados na próxima imagem):</span><span class="sxs-lookup"><span data-stu-id="e2ffd-128">You can click on the **Update All** button or you can search for only OWIN packages (shown in the next image):</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image5.png)

<span data-ttu-id="e2ffd-129">A imagem abaixo, são mostrados apenas os pacotes OWIN:</span><span class="sxs-lookup"><span data-stu-id="e2ffd-129">In the image below, only OWIN packages are shown:</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image6.png)

<span data-ttu-id="e2ffd-130">Do pacote Manager Console (PMC), você pode inserir o `Update-Package` comando, que atualizará todos os pacotes.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-130">From the Package Manager Console (PMC), you can enter the `Update-Package` command, which will update all packages.</span></span>

<span data-ttu-id="e2ffd-131">Pressione **F5** ou **Ctrl + F5** para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-131">Press **F5** or **Ctrl+F5** to run the application.</span></span> <span data-ttu-id="e2ffd-132">Na imagem abaixo, o número da porta for 1234.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-132">In the image below, the port number is 1234.</span></span> <span data-ttu-id="e2ffd-133">Quando você executa o aplicativo, você verá um número de porta diferente.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-133">When you run the application, you'll see a different port number.</span></span>

<span data-ttu-id="e2ffd-134">Dependendo do tamanho da janela do navegador, talvez seja necessário clicar no ícone de navegação para ver os **página inicial**, **sobre**, **contato**, **registrar**e **efetuar login** links.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-134">Depending on the size of your browser window, you might need to click the navigation icon to see the **Home**, **About**, **Contact**, **Register** and **Log in** links.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image7.png)  
![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image8.png) 

<a id="ssl"></a>
## <a name="setting-up-ssl-in-the-project"></a><span data-ttu-id="e2ffd-135">Configurar o SSL no projeto</span><span class="sxs-lookup"><span data-stu-id="e2ffd-135">Setting up SSL in the Project</span></span>

<span data-ttu-id="e2ffd-136">Para se conectar a provedores de autenticação, como Google e Facebook, você precisará configurar o IIS Express para usar SSL.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-136">To connect to authentication providers like Google and Facebook, you will need to set up IIS-Express to use SSL.</span></span> <span data-ttu-id="e2ffd-137">É importante manter usando SSL, após o logon e não descarte volta HTTP, o cookie de logon é apenas como segredo como seu nome de usuário e senha e sem o uso de SSL que você está enviando em texto não criptografado na transmissão.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-137">It's important to keep using SSL after login and not drop back to HTTP, your login cookie is just as secret as your username and password, and without using SSL you're sending it in clear-text across the wire.</span></span> <span data-ttu-id="e2ffd-138">Além disso, você já passou um tempo para executar o handshake e proteja o canal (que é a maior parte do que o torna mais lento do que o HTTP do HTTPS) antes que o pipeline MVC é executado, então redirecionar de volta para o HTTP depois que você está conectado não fará a solicitação atual ou futuro solicitações muito mais rápidas.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-138">Besides, you've already taken the time to perform the handshake and secure the channel (which is the bulk of what makes HTTPS slower than HTTP) before the MVC pipeline is run, so redirecting back to HTTP after you're logged in won't make the current request or future requests much faster.</span></span>

1. <span data-ttu-id="e2ffd-139">Na **Gerenciador de soluções**, clique no **MvcAuth** projeto.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-139">In **Solution Explorer**, click the **MvcAuth** project.</span></span>
2. <span data-ttu-id="e2ffd-140">Pressione a tecla F4 para mostrar as propriedades do projeto.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-140">Hit the F4 key to show the project properties.</span></span> <span data-ttu-id="e2ffd-141">Como alternativa, a partir de **exibição** menu você pode selecionar **janela propriedades**.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-141">Alternatively, from the **View** menu you can select **Properties Window**.</span></span>
3. <span data-ttu-id="e2ffd-142">Alteração **SSL habilitado** como True.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-142">Change **SSL Enabled** to True.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image9.png)
4. <span data-ttu-id="e2ffd-143">Copie a URL do SSL (que será `https://localhost:44300/` , a menos que você criou outros projetos SSL).</span><span class="sxs-lookup"><span data-stu-id="e2ffd-143">Copy the SSL URL (which will be `https://localhost:44300/` unless you've created other SSL projects).</span></span>
5. <span data-ttu-id="e2ffd-144">Na **Gerenciador de soluções**, clique com botão direito do **MvcAuth** do projeto e selecione **propriedades**.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-144">In **Solution Explorer**, right click the **MvcAuth** project and select **Properties**.</span></span>
6. <span data-ttu-id="e2ffd-145">Selecione o **Web** guia e, em seguida, cole a URL do SSL para o **Url do projeto** caixa.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-145">Select the **Web** tab, and then paste the SSL URL into the **Project Url** box.</span></span> <span data-ttu-id="e2ffd-146">Salve o arquivo (Ctl + S).</span><span class="sxs-lookup"><span data-stu-id="e2ffd-146">Save the file (Ctl+S).</span></span> <span data-ttu-id="e2ffd-147">Você precisará dessa URL para configurar os aplicativos de autenticação do Facebook e Google.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-147">You will need this URL to configure Facebook and Google authentication apps.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image10.png)
7. <span data-ttu-id="e2ffd-148">Adicione a [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) atributo para o `Home` controlador para exigir que todas as solicitações deve usar HTTPS.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-148">Add the [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) attribute to the `Home` controller to require all requests must use HTTPS.</span></span> <span data-ttu-id="e2ffd-149">Uma abordagem mais segura é adicionar o [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) filtro para o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-149">A more secure approach is to add the [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) filter to the application.</span></span> <span data-ttu-id="e2ffd-150">Consulte a seção &quot;proteger o aplicativo com SSL e o atributo Authorize&quot; no meu tutoral [criar um aplicativo ASP.NET MVC com autenticação e o banco de dados SQL e implantar no serviço de aplicativo do Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="e2ffd-150">See the section &quot;Protect the Application with SSL and the Authorize Attribute&quot; in my tutoral [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span> <span data-ttu-id="e2ffd-151">Abaixo está uma parte do controlador Home.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-151">A portion of the Home controller is shown below.</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample1.cs?highlight=1)]
8. <span data-ttu-id="e2ffd-152">Pressione CTRL+F5 para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-152">Press CTRL+F5 to run the application.</span></span> <span data-ttu-id="e2ffd-153">Se você tiver instalado o certificado no passado, você pode ignorar o restante desta seção e vá para [criando um aplicativo do Google para o OAuth 2 e conectar o aplicativo ao projeto](#goog), caso contrário, siga as instruções para confiar a autoassinado certificado que o IIS Express gerou.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-153">If you've installed the certificate in the past, you can skip the rest of this section and jump to [Creating a Google app for OAuth 2 and connecting the app to the project](#goog), otherwise, follow the instructions to trust the self-signed certificate that IIS Express has generated.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image11.png)
9. <span data-ttu-id="e2ffd-154">Leia as **aviso de segurança** caixa de diálogo e clique **Sim** se você quiser instalar o certificado que representa o localhost.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-154">Read the **Security Warning** dialog and then click **Yes** if you want to install the certificate representing localhost.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image12.png)
10. <span data-ttu-id="e2ffd-155">IE mostra a *Home* página e não há nenhum aviso de SSL.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-155">IE shows the *Home* page and there are no SSL warnings.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image13.png)
11. <span data-ttu-id="e2ffd-156">Google Chrome também aceita o certificado e mostrará o conteúdo HTTPS sem aviso.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-156">Google Chrome also accepts the certificate and will show HTTPS content without a warning.</span></span> <span data-ttu-id="e2ffd-157">O Firefox usa seu próprio repositório de certificados, portanto, ele exibirá um aviso.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-157">Firefox uses its own certificate store, so it will display a warning.</span></span> <span data-ttu-id="e2ffd-158">Para nosso aplicativo você pode clicar com segurança **compreende os riscos**.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-158">For our application you can safely click **I Understand the Risks**.</span></span>   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image14.png)

<a id="goog"></a>
## <a name="creating-a-google-app-for-oauth-2-and-connecting-the-app-to-the-project"></a><span data-ttu-id="e2ffd-159">Criando um aplicativo do Google para o OAuth 2 e conectar o aplicativo ao projeto</span><span class="sxs-lookup"><span data-stu-id="e2ffd-159">Creating a Google app for OAuth 2 and connecting the app to the project</span></span>

> [!WARNING]
> <span data-ttu-id="e2ffd-160">Para instruções de OAuth do Google atuais, consulte [Google Configurando a autenticação no ASP.NET Core](/aspnet/core/security/authentication/social/google-logins).</span><span class="sxs-lookup"><span data-stu-id="e2ffd-160">For current Google OAuth instructions, see [Configuring Google authentication in ASP.NET Core](/aspnet/core/security/authentication/social/google-logins).</span></span>

1. <span data-ttu-id="e2ffd-161">Navegue até a [Console de desenvolvedores do Google](https://console.developers.google.com/).</span><span class="sxs-lookup"><span data-stu-id="e2ffd-161">Navigate to the [Google Developers Console](https://console.developers.google.com/).</span></span>
2. <span data-ttu-id="e2ffd-162">Se você não tiver criado um projeto antes, selecione **credenciais** na guia à esquerda e em seguida, selecione **criar**.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-162">If you haven't created a project before, select **Credentials** in the left tab, and then select **Create**.</span></span>
3. <span data-ttu-id="e2ffd-163">Na guia à esquerda, clique em **credenciais**.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-163">In the left tab, click **Credentials**.</span></span>
4. <span data-ttu-id="e2ffd-164">Clique em **criar credenciais** , em seguida, **ID do cliente OAuth**.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-164">Click **Create credentials** then **OAuth client ID**.</span></span> 

    1. <span data-ttu-id="e2ffd-165">No **criar ID do cliente** caixa de diálogo, mantenha o padrão **aplicativo Web** para o tipo de aplicativo.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-165">In the **Create Client ID** dialog, keep the default **Web application** for the application type.</span></span>
    2. <span data-ttu-id="e2ffd-166">Defina as **JavaScript autorizadas** origens para a URL do SSL usado acima (`https://localhost:44300/` , a menos que você criou outros projetos SSL)</span><span class="sxs-lookup"><span data-stu-id="e2ffd-166">Set the **Authorized JavaScript** origins to the SSL URL you used above (`https://localhost:44300/` unless you've created other SSL projects)</span></span>
    3. <span data-ttu-id="e2ffd-167">Defina as **URI de redirecionamento autorizado** para:</span><span class="sxs-lookup"><span data-stu-id="e2ffd-167">Set the **Authorized redirect URI** to:</span></span>  
         `https://localhost:44300/signin-google`
5. <span data-ttu-id="e2ffd-168">Clique no item de menu de tela de consentimento de OAuth e, em seguida, definir seu nome de produto e o endereço de email.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-168">Click the OAuth Consent screen menu item, then set your email address and product name.</span></span> <span data-ttu-id="e2ffd-169">Quando você tiver concluído o formulário, clique **salvar**.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-169">When you have completed the form click **Save**.</span></span>
6. <span data-ttu-id="e2ffd-170">Clique no item de menu de biblioteca, pesquise **API do Google +**, clique nele e em seguida, pressione Enable.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-170">Click the Library menu item, search **Google+ API**, click on it then press Enable.</span></span>
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image15.png)  
  
   <span data-ttu-id="e2ffd-171">A imagem abaixo mostra as APIs habilitadas.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-171">The image below shows the enabled APIs.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image16.png)
7. <span data-ttu-id="e2ffd-172">No Gerenciador de API de APIs do Google, visite o **credenciais** tab para obter o **ID do cliente**.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-172">From the Google APIs API Manager, visit the **Credentials** tab to obtain the **Client ID**.</span></span> <span data-ttu-id="e2ffd-173">Download para salvar um arquivo JSON com segredos do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-173">Download to save a JSON file with application secrets.</span></span> <span data-ttu-id="e2ffd-174">Copie e cole a **ClientId** e **ClientSecret** no `UseGoogleAuthentication` método encontrado no *Startup.Auth.cs* arquivo no *App_Start* pasta.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-174">Copy and paste the **ClientId** and **ClientSecret** into the `UseGoogleAuthentication` method found in the *Startup.Auth.cs* file in the *App_Start* folder.</span></span> <span data-ttu-id="e2ffd-175">O **ClientId** e **ClientSecret** valores mostrados abaixo são exemplos e não funcionam.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-175">The **ClientId** and **ClientSecret** values shown below are samples and don't work.</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample2.cs?highlight=37-39)]

    > [!WARNING]
    > <span data-ttu-id="e2ffd-176">Security - nunca armazenar os dados confidenciais em seu código-fonte.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-176">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="e2ffd-177">A conta e as credenciais são adicionadas ao código acima para manter o exemplo simples.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-177">The account and credentials are added to the code above to keep the sample simple.</span></span> <span data-ttu-id="e2ffd-178">Ver [práticas recomendadas para implantar senhas e outros dados confidenciais no ASP.NET e o serviço de aplicativo do Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span><span class="sxs-lookup"><span data-stu-id="e2ffd-178">See [Best practices for deploying passwords and other sensitive data to ASP.NET and Azure App Service](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span></span>
8. <span data-ttu-id="e2ffd-179">Pressione **CTRL + F5** para compilar e executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-179">Press **CTRL+F5** to build and run the application.</span></span> <span data-ttu-id="e2ffd-180">Clique o **faça logon no** link.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-180">Click the **Log in** link.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image17.png)
9. <span data-ttu-id="e2ffd-181">Sob **usar outro serviço para fazer logon**, clique em **Google**.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-181">Under **Use another service to log in**, click **Google**.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image18.png)

    > [!NOTE]
    > <span data-ttu-id="e2ffd-182">Se você perder qualquer uma das etapas acima, você receberá um erro HTTP 401.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-182">If you miss any of the steps above you will get a HTTP 401 error.</span></span> <span data-ttu-id="e2ffd-183">Verifique novamente as etapas acima.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-183">Recheck your steps above.</span></span> <span data-ttu-id="e2ffd-184">Se você perder uma configuração necessária (por exemplo **nome do produto**), adicione o item ausente e salve; pode levar alguns minutos para que a autenticação funcione.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-184">If you miss a required setting (for example **product name**), add the missing item and save; it can take a few minutes for authentication to work.</span></span>
10. <span data-ttu-id="e2ffd-185">Você será redirecionado ao site do Google, onde você vai inserir suas credenciais.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-185">You will be redirected to the Google site where you will enter your credentials.</span></span>   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image19.png)
11. <span data-ttu-id="e2ffd-186">Depois de inserir suas credenciais, você será solicitado a conceder permissões para o aplicativo web que você acabou de criar:</span><span class="sxs-lookup"><span data-stu-id="e2ffd-186">After you enter your credentials, you will be prompted to give permissions to the web application you just created:</span></span>
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image20.png)
12. <span data-ttu-id="e2ffd-187">Clique em **aceitar**.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-187">Click **Accept**.</span></span> <span data-ttu-id="e2ffd-188">Você será redirecionado para o **registrar** página do aplicativo MvcAuth onde você pode registrar sua conta do Google.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-188">You will now be redirected back to the **Register** page of the MvcAuth application where you can register your Google account.</span></span> <span data-ttu-id="e2ffd-189">Você tem a opção de alterar o nome de registro de email local utilizado para sua conta do Gmail, mas você geralmente deseja manter o alias de email padrão (ou seja, aquele usado para autenticação).</span><span class="sxs-lookup"><span data-stu-id="e2ffd-189">You have the option of changing the local email registration name used for your Gmail account, but you generally want to keep the default email alias (that is, the one you used for authentication).</span></span> <span data-ttu-id="e2ffd-190">Clique em **Registrar**.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-190">Click **Register**.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image21.png)

<a id="fb"></a>
## <a name="creating-the-app-in-facebook-and-connecting-the-app-to-the-project"></a><span data-ttu-id="e2ffd-191">Criando o aplicativo no Facebook e conectar o aplicativo ao projeto</span><span class="sxs-lookup"><span data-stu-id="e2ffd-191">Creating the app in Facebook and connecting the app to the project</span></span>

> [!WARNING]
> <span data-ttu-id="e2ffd-192">Para instruções de autenticação do Facebook OAuth2 atuais, consulte [autenticação do Facebook Configurando](/aspnet/core/security/authentication/social/facebook-logins)</span><span class="sxs-lookup"><span data-stu-id="e2ffd-192">For current Facebook OAuth2 authentication instructions, see [Configuring Facebook authentication](/aspnet/core/security/authentication/social/facebook-logins)</span></span>


<a id="mdb"></a>
## <a name="examine-the-membership-data"></a><span data-ttu-id="e2ffd-193">Examinar os dados de associação</span><span class="sxs-lookup"><span data-stu-id="e2ffd-193">Examine the Membership Data</span></span>

<span data-ttu-id="e2ffd-194">No **modo de exibição** menu, clique em **Gerenciador de servidores**.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-194">In the **View** menu, click **Server Explorer**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image32.png)

<span data-ttu-id="e2ffd-195">Expandir **DefaultConnection (MvcAuth)**, expanda **tabelas**, clique com botão direito **AspNetUsers** e clique em **Mostrar dados da tabela**.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-195">Expand **DefaultConnection (MvcAuth)**, expand **Tables**, right click **AspNetUsers** and click **Show Table Data**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image33.png)

![dados da tabela aspnetusers](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image34.png)

<a id="ap"></a>
## <a name="adding-profile-data-to-the-user-class"></a><span data-ttu-id="e2ffd-197">Adicionar dados de perfil para a classe de usuário</span><span class="sxs-lookup"><span data-stu-id="e2ffd-197">Adding Profile Data to the User Class</span></span>

<span data-ttu-id="e2ffd-198">Nesta seção você adicionará data de nascimento e cidade para os dados do usuário durante o registro, conforme mostrado na imagem a seguir.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-198">In this section you'll add birth date and home town to the user data during registration, as shown in the following image.</span></span>

![reg com a cidade e Bday](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image35.png)

<span data-ttu-id="e2ffd-200">Abra o *Models\IdentityModels.cs* arquivo e adicione as propriedades de cidade de data e a página inicial de nascimento:</span><span class="sxs-lookup"><span data-stu-id="e2ffd-200">Open the *Models\IdentityModels.cs* file and add birth date and home town properties:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample4.cs?highlight=3-4)]

<span data-ttu-id="e2ffd-201">Abra o *Models\AccountViewModels.cs* de arquivo e o conjunto de propriedades de cidade de data e a página inicial de nascimento `ExternalLoginConfirmationViewModel`.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-201">Open the *Models\AccountViewModels.cs* file and the set birth date and home town properties in `ExternalLoginConfirmationViewModel`.</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample5.cs?highlight=8-9)]

<span data-ttu-id="e2ffd-202">Abra o *Controllers\AccountController.cs* arquivo e adicione o código para a cidade de data e a página inicial de nascimento do `ExternalLoginConfirmation` método de ação, conforme mostrado:</span><span class="sxs-lookup"><span data-stu-id="e2ffd-202">Open the *Controllers\AccountController.cs* file and add code for birth date and home town in the `ExternalLoginConfirmation` action method as shown:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample6.cs?highlight=21-23)]

<span data-ttu-id="e2ffd-203">Adicionar data de nascimento e cidade para os *Views\Account\ExternalLoginConfirmation.cshtml* arquivo:</span><span class="sxs-lookup"><span data-stu-id="e2ffd-203">Add birth date and home town to the *Views\Account\ExternalLoginConfirmation.cshtml* file:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample7.cshtml?highlight=27-40)]

<span data-ttu-id="e2ffd-204">Exclua o banco de dados de associação para que você pode registrar sua conta do Facebook com seu aplicativo novamente e verifique se que você pode adicionar a nova data de nascimento e informações de perfil de cidade.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-204">Delete the membership database so you can again register your Facebook account with your application and verify you can add the new birth date and home town profile information.</span></span>

<span data-ttu-id="e2ffd-205">Da **Gerenciador de soluções**, clique no **Show All Files** ícone e o botão direito do mouse *Add\_Data\aspnet-MvcAuth -&lt;carimbo de data&gt;. mdf* e clique em **excluir**.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-205">From **Solution Explorer**, click the **Show All Files** icon, then right click *Add\_Data\aspnet-MvcAuth-&lt;dateStamp&gt;.mdf* and click **Delete**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image36.png)

<span data-ttu-id="e2ffd-206">Dos **ferramentas** menu, clique em **Gerenciador de pacotes NuGet**, em seguida, clique em **Package Manager Console** (PMC).</span><span class="sxs-lookup"><span data-stu-id="e2ffd-206">From the **Tools** menu, click **NuGet Package Manger**, then click **Package Manager Console** (PMC).</span></span> <span data-ttu-id="e2ffd-207">Digite os seguintes comandos no PMC.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-207">Enter the following commands in the PMC.</span></span>

1. <span data-ttu-id="e2ffd-208">Enable-Migrations</span><span class="sxs-lookup"><span data-stu-id="e2ffd-208">Enable-Migrations</span></span>
2. <span data-ttu-id="e2ffd-209">Add-Migration Init</span><span class="sxs-lookup"><span data-stu-id="e2ffd-209">Add-Migration Init</span></span>
3. <span data-ttu-id="e2ffd-210">Atualizar banco de dados</span><span class="sxs-lookup"><span data-stu-id="e2ffd-210">Update-Database</span></span>

<span data-ttu-id="e2ffd-211">Execute o aplicativo e usar o FaceBook e Google para fazer logon e registrar alguns usuários.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-211">Run the application and use FaceBook and Google to log in and register some users.</span></span>

## <a name="examine-the-membership-data"></a><span data-ttu-id="e2ffd-212">Examinar os dados de associação</span><span class="sxs-lookup"><span data-stu-id="e2ffd-212">Examine the Membership Data</span></span>

<span data-ttu-id="e2ffd-213">No **modo de exibição** menu, clique em **Gerenciador de servidores**.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-213">In the **View** menu, click **Server Explorer**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image37.png)

<span data-ttu-id="e2ffd-214">Clique com botão direito **AspNetUsers** e clique em **Mostrar dados da tabela**.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-214">Right click **AspNetUsers** and click **Show Table Data**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image38.png)

<span data-ttu-id="e2ffd-215">O `HomeTown` e `BirthDate` campos são mostrados abaixo.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-215">The `HomeTown` and `BirthDate` fields are shown below.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image39.png)

<a id="off"></a>
## <a name="logging-off-your-app-and-logging-in-with-another-account"></a><span data-ttu-id="e2ffd-216">Fazer logoff do seu aplicativo e fazendo logon com outra conta</span><span class="sxs-lookup"><span data-stu-id="e2ffd-216">Logging off your App and Logging in With Another Account</span></span>

<span data-ttu-id="e2ffd-217">Se fazer logon no seu aplicativo com o Facebook e, em seguida, faça logoff e tente fazer logon novamente com uma conta diferente do Facebook (usando o mesmo navegador), você será imediatamente conectado à conta do Facebook anterior, que você usou.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-217">If you log on to your app with Facebook,, and then log out and try to log in again with a different Facebook account (using the same browser), you will be immediately logged in to the previous Facebook account you used.</span></span> <span data-ttu-id="e2ffd-218">Para usar outra conta, você precisa navegar para o Facebook e faça logoff no Facebook.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-218">In order to use another account, you need to navigate to Facebook and log out at Facebook.</span></span> <span data-ttu-id="e2ffd-219">A mesma regra se aplica a qualquer outro provedor do autenticação de terceiros 3ª.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-219">The same rule applies to any other 3rd party authentication provider.</span></span> <span data-ttu-id="e2ffd-220">Como alternativa, você pode fazer logon com outra conta usando um navegador diferente.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-220">Alternatively, you can log in with another account by using a different browser.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e2ffd-221">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="e2ffd-221">Next Steps</span></span>

<span data-ttu-id="e2ffd-222">Ver [apresentando os provedores de segurança do Yahoo e do OAuth do LinkedIn para OWIN](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) por Jerrie Pelser para obter instruções do Yahoo e LinkedIn.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-222">See [Introducing the Yahoo and LinkedIn OAuth security providers for OWIN](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) by Jerrie Pelser for Yahoo and LinkedIn instructions.</span></span> <span data-ttu-id="e2ffd-223">Consulte do Jerrie praticamente botões de logon social para ASP.NET MVC 5 obter os botões de logon social enable.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-223">See Jerrie's Pretty social login buttons for ASP.NET MVC 5 to get enable social login buttons.</span></span>

<span data-ttu-id="e2ffd-224">Siga o tutorial [criar um aplicativo ASP.NET MVC com autenticação e o banco de dados SQL e implantar no serviço de aplicativo do Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data), que continua a este tutorial e mostra o seguinte:</span><span class="sxs-lookup"><span data-stu-id="e2ffd-224">Follow my tutorial [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data), which continues this tutorial and shows the following:</span></span>

1. <span data-ttu-id="e2ffd-225">Como implantar seu aplicativo no Azure.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-225">How to deploy your app to Azure.</span></span>
2. <span data-ttu-id="e2ffd-226">Como proteger seu aplicativo com as funções.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-226">How to secure you app with roles.</span></span>
3. <span data-ttu-id="e2ffd-227">Como proteger seu aplicativo com o [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx) e [autorizar](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx) filtros.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-227">How to secure your app with the [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx) and [Authorize](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx) filters.</span></span>
4. <span data-ttu-id="e2ffd-228">Como usar a API de associação para adicionar usuários e funções.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-228">How to use the membership API to add users and roles.</span></span>

<span data-ttu-id="e2ffd-229">Deixe comentários sobre como você gostou neste tutorial e o que poderíamos melhorar.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-229">Please leave feedback on how you liked this tutorial and what we could improve.</span></span> <span data-ttu-id="e2ffd-230">Você também pode solicitar novos tópicos em [Mostrar-Me como com código](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).</span><span class="sxs-lookup"><span data-stu-id="e2ffd-230">You can also request new topics at [Show Me How With Code](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).</span></span> <span data-ttu-id="e2ffd-231">Você pode até mesmo solicitar e votar em novos recursos a serem adicionadas ao ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-231">You can even ask for and vote on new features to be added to ASP.NET.</span></span> <span data-ttu-id="e2ffd-232">Por exemplo, você poderá votar para uma ferramenta de [criar e gerenciar usuários e funções.](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)</span><span class="sxs-lookup"><span data-stu-id="e2ffd-232">For example, you can vote for a tool to [create and manage users and roles.](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)</span></span>

<span data-ttu-id="e2ffd-233">Para uma boa explicação de como funcionam os serviços de autenticação externos do ASP.NET, consulte de Robert McMurray [serviços de autenticação externos](https://asp.net/web-api/overview/security/external-authentication-services).</span><span class="sxs-lookup"><span data-stu-id="e2ffd-233">For an good explanation of how ASP.NET External Authentication Services work, see Robert McMurray's [External Authentication Services](https://asp.net/web-api/overview/security/external-authentication-services).</span></span> <span data-ttu-id="e2ffd-234">O artigo de Robert também entra em detalhes na habilitação da autenticação da Microsoft e Twitter.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-234">Robert's article also goes into detail in enabling Microsoft and Twitter authentication.</span></span> <span data-ttu-id="e2ffd-235">Tom Dykstra do excelente [tutorial do EF/MVC](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) mostra como trabalhar com o Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e2ffd-235">Tom Dykstra's excellent [EF/MVC tutorial](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) shows how to work with the Entity Framework.</span></span>
