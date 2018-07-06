---
uid: web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
title: Fazer logon usando Sites externos em uma Web do ASP.NET (Razor) sites de páginas | Microsoft Docs
author: tfitzmac
description: Este artigo explica como fazer logon seu site de páginas da Web do ASP.NET (Razor) usando o Facebook, Google, Twitter, Yahoo e outros sites — ou seja, como dar suporte a...
ms.author: aspnetcontent
ms.date: 02/21/2014
ms.assetid: ef852096-a5bf-47b3-9945-125cde065093
msc.legacyurl: /web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: e6c5b578b4c74fd04136d31751d51b59ba9197ba
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37825431"
---
<a name="logging-in-using-external-sites-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="13da6-103">Fazer logon usando Sites externos em um Site do ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="13da6-103">Logging In Using External Sites in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="13da6-104">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="13da6-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="13da6-105">Este artigo explica como fazer logon seu site de páginas da Web do ASP.NET (Razor) usando o Facebook, Google, Twitter, Yahoo e outros sites — ou seja, como dar suporte a OAuth e OpenID no seu site.</span><span class="sxs-lookup"><span data-stu-id="13da6-105">This article explains how to log in to your ASP.NET Web Pages (Razor) site using Facebook, Google, Twitter, Yahoo, and other sites — that is, how to support OAuth and OpenID in your site.</span></span>
> 
> <span data-ttu-id="13da6-106">O que você aprenderá:</span><span class="sxs-lookup"><span data-stu-id="13da6-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="13da6-107">Como habilitar o logon de outros sites quando você usa o modelo de Site do WebMatrix inicial.</span><span class="sxs-lookup"><span data-stu-id="13da6-107">How to enable login from other sites when you use the WebMatrix Starter Site template.</span></span>
> 
> <span data-ttu-id="13da6-108">Esse é o recurso ASP.NET introduzido no artigo:</span><span class="sxs-lookup"><span data-stu-id="13da6-108">This is the ASP.NET feature introduced in the article:</span></span>
> 
> - <span data-ttu-id="13da6-109">O `OAuthWebSecurity` auxiliar.</span><span class="sxs-lookup"><span data-stu-id="13da6-109">The `OAuthWebSecurity` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="13da6-110">Versões de software usadas no tutorial</span><span class="sxs-lookup"><span data-stu-id="13da6-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="13da6-111">Páginas da Web do ASP.NET (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="13da6-111">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="13da6-112">O WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="13da6-112">WebMatrix 3</span></span>

<span data-ttu-id="13da6-113">Páginas da Web ASP.NET inclui suporte para [OAuth](http://oauth.net/) e [OpenID](http://openid.net/) provedores.</span><span class="sxs-lookup"><span data-stu-id="13da6-113">ASP.NET Web Pages includes support for [OAuth](http://oauth.net/) and [OpenID](http://openid.net/) providers.</span></span> <span data-ttu-id="13da6-114">Usando esses provedores, você pode permitir que os usuários façam logon em seu site usando as credenciais existentes do Facebook, Twitter, Microsoft e Google.</span><span class="sxs-lookup"><span data-stu-id="13da6-114">Using these providers, you can let users log into your site using their existing credentials from Facebook, Twitter, Microsoft, and Google.</span></span> <span data-ttu-id="13da6-115">Por exemplo, para fazer logon usando uma conta do Facebook, os usuários podem escolher apenas um ícone de Facebook, que redireciona para a página de logon do Facebook em que eles inserirem suas informações de usuário.</span><span class="sxs-lookup"><span data-stu-id="13da6-115">For example, to log in using a Facebook account, users can just choose a Facebook icon, which redirects them to the Facebook login page where they enter their user information.</span></span> <span data-ttu-id="13da6-116">Em seguida, podem associar o logon do Facebook com sua conta em seu site.</span><span class="sxs-lookup"><span data-stu-id="13da6-116">They can then associate the Facebook login with their account on your site.</span></span> <span data-ttu-id="13da6-117">Um aprimoramento relacionado aos recursos de associação de páginas da Web é que os usuários podem associar vários logons (incluindo logons de sites de rede social) com uma única conta em seu site.</span><span class="sxs-lookup"><span data-stu-id="13da6-117">A related enhancement to the Web Pages membership features is that users can associate multiple logins (including logins from social networking sites) with a single account on your website.</span></span>

<span data-ttu-id="13da6-118">Esta imagem mostra a página de logon a partir de **Starter Site** modelo, em que um usuário pode escolher um ícone do Facebook, Twitter, Google ou Microsoft para habilitar o registro em log com uma conta externa:</span><span class="sxs-lookup"><span data-stu-id="13da6-118">This image shows the Login page from the **Starter Site** template, where a user can choose a Facebook, Twitter, Google or Microsoft icon to enable logging in with an external account:</span></span>

![provedores externos](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image1.png)

<span data-ttu-id="13da6-120">Você pode habilitar a associação de OAuth e OpenID removendo algumas linhas de código na **Starter Site** modelo.</span><span class="sxs-lookup"><span data-stu-id="13da6-120">You can enable OAuth and OpenID membership by uncommenting a few lines of code in the **Starter Site** template.</span></span> <span data-ttu-id="13da6-121">Os métodos e propriedades que você usa para trabalhar com o OAuth e OpenID provedores estão no `WebMatrix.Security.OAuthWebSecurity` classe.</span><span class="sxs-lookup"><span data-stu-id="13da6-121">The methods and properties you use to work with the OAuth and OpenID providers are in the `WebMatrix.Security.OAuthWebSecurity` class.</span></span> <span data-ttu-id="13da6-122">O **Starter Site** modelo inclui uma infraestrutura completa de associação, completo com uma página de logon, um banco de dados de associação e todo o código necessário permitir que os usuários façam logon em seu site usando credenciais locais ou aqueles de outro site .</span><span class="sxs-lookup"><span data-stu-id="13da6-122">The **Starter Site** template includes a full membership infrastructure, complete with a login page, a membership database, and all the code you need to let users log into your site using either local credentials or those from another site.</span></span>

<span data-ttu-id="13da6-123">Esta seção fornece um exemplo de como permitir que usuários logon de sites externos a um site que se baseia o **Starter Site** modelo.</span><span class="sxs-lookup"><span data-stu-id="13da6-123">This section provides an example of how to let users log in from external sites to a site that's based on the **Starter Site** template.</span></span> <span data-ttu-id="13da6-124">Depois de criar um site inicial, você tanto (detalhes):</span><span class="sxs-lookup"><span data-stu-id="13da6-124">After creating a starter site, you do this (details follow):</span></span>

- <span data-ttu-id="13da6-125">Para os sites que usam um provedor de OAuth (Facebook, Twitter e Microsoft), você pode criar um aplicativo no site externo.</span><span class="sxs-lookup"><span data-stu-id="13da6-125">For the sites that use an OAuth provider (Facebook, Twitter, and Microsoft), you create an application on the external site.</span></span> <span data-ttu-id="13da6-126">Isso lhe dá as chaves de aplicativo que são necessários para invocar o recurso de logon para esses sites.</span><span class="sxs-lookup"><span data-stu-id="13da6-126">This gives you application keys that you'll need in order to invoke the login feature for those sites.</span></span>
- <span data-ttu-id="13da6-127">Para sites que usam um provedor do OpenID (Google), você não precisa criar um aplicativo.</span><span class="sxs-lookup"><span data-stu-id="13da6-127">For sites that use an OpenID provider (Google), you do not have to create an application.</span></span> <span data-ttu-id="13da6-128">Todos esses sites, você deve ter uma conta para fazer logon e para criar aplicativos de desenvolvedor.</span><span class="sxs-lookup"><span data-stu-id="13da6-128">For all of these sites, you must have an account in order to log in and to create developer applications.</span></span>

    > [!NOTE]
    > <span data-ttu-id="13da6-129">Aplicativos da Microsoft só aceitam uma URL ao vivo para um site de trabalho, portanto você não pode usar uma URL do site local para logons de teste.</span><span class="sxs-lookup"><span data-stu-id="13da6-129">Microsoft applications only accept a live URL for a working website, so you cannot use a local website URL for testing logins.</span></span>
- <span data-ttu-id="13da6-130">Edite alguns arquivos no seu site para especificar o provedor de autenticação apropriado e enviar um logon para o site que você deseja usar.</span><span class="sxs-lookup"><span data-stu-id="13da6-130">Edit a few files in your website in order to specify the appropriate authentication provider and to submit a login to the site you want to use.</span></span>

<span data-ttu-id="13da6-131">Este artigo fornece instruções separadas para as seguintes tarefas:</span><span class="sxs-lookup"><span data-stu-id="13da6-131">This article provides separate instructions for the following tasks:</span></span>

- [<span data-ttu-id="13da6-132">Habilitar os logons do Google</span><span class="sxs-lookup"><span data-stu-id="13da6-132">Enabling Google logins</span></span>](#To_enable_Google_logins)
- [<span data-ttu-id="13da6-133">Habilitar os logons do Facebook</span><span class="sxs-lookup"><span data-stu-id="13da6-133">Enabling Facebook logins</span></span>](#To_enable_Facebook_logins)
- [<span data-ttu-id="13da6-134">Habilitar os logons do Twitter</span><span class="sxs-lookup"><span data-stu-id="13da6-134">Enabling Twitter logins</span></span>](#To_enable_Twitter_logins)

<a id="To_enable_Google_logins"></a>
## <a name="enabling-google-logins"></a><span data-ttu-id="13da6-135">Habilitar os logons do Google</span><span class="sxs-lookup"><span data-stu-id="13da6-135">Enabling Google Logins</span></span>

1. <span data-ttu-id="13da6-136">Criar ou abrir um site de páginas da Web ASP.NET com base no modelo de Site do WebMatrix inicial.</span><span class="sxs-lookup"><span data-stu-id="13da6-136">Create or open an ASP.NET Web Pages site that's based on the WebMatrix Starter Site template.</span></span>
2. <span data-ttu-id="13da6-137">Abra o  *\_AppStart.cshtml* página e remova a seguinte linha de código.</span><span class="sxs-lookup"><span data-stu-id="13da6-137">Open the *\_AppStart.cshtml* page and uncomment the following line of code.</span></span> 

    [!code-css[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample1.css)]

### <a name="testing-google-login"></a><span data-ttu-id="13da6-138">Testar o logon do Google</span><span class="sxs-lookup"><span data-stu-id="13da6-138">Testing Google login</span></span>

1. <span data-ttu-id="13da6-139">Execute o *default. cshtml* página do seu site e escolha o **faça logon no** botão.</span><span class="sxs-lookup"><span data-stu-id="13da6-139">Run the *default.cshtml* page of your site and choose the **Log in** button.</span></span>
2. <span data-ttu-id="13da6-140">No *logon* página, o **Use outro serviço para fazer logon** , escolha qualquer um o **Google** ou **Yahoo** botão Enviar.</span><span class="sxs-lookup"><span data-stu-id="13da6-140">On the *Login* page, in the **Use another service to log in** section, choose either the **Google** or **Yahoo** submit button.</span></span> <span data-ttu-id="13da6-141">Este exemplo usa o logon do Google.</span><span class="sxs-lookup"><span data-stu-id="13da6-141">This example uses the Google login.</span></span> 

    <span data-ttu-id="13da6-142">A página da web redireciona a solicitação para a página de logon do Google.</span><span class="sxs-lookup"><span data-stu-id="13da6-142">The web page redirects the request to the Google login page.</span></span>

    ![Entrar do Google](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image2.png)
3. <span data-ttu-id="13da6-144">Insira as credenciais para uma conta existente do Google.</span><span class="sxs-lookup"><span data-stu-id="13da6-144">Enter credentials for an existing Google account.</span></span>
4. <span data-ttu-id="13da6-145">Se o Google pede que você queira permitir *Localhost* para usar as informações da conta, clique em **permitir**.</span><span class="sxs-lookup"><span data-stu-id="13da6-145">If Google asks you whether you want to allow *Localhost* to use information from the account, click **Allow**.</span></span>

    <span data-ttu-id="13da6-146">O código usa o token do Google para autenticar o usuário e, em seguida, retorna a esta página em seu site.</span><span class="sxs-lookup"><span data-stu-id="13da6-146">The code uses the Google token to authenticate the user, and then returns to this page on your website.</span></span> <span data-ttu-id="13da6-147">Esta página permite que os usuários a associar seu logon do Google com uma conta existente no seu site, ou eles podem registrar uma nova conta no seu site para associar o logon externo com.</span><span class="sxs-lookup"><span data-stu-id="13da6-147">This page lets users associate their Google login with an existing account on your website, or they can register a new account on your site to associate the external login with.</span></span>

    ![OAuth 5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image3.png)
5. <span data-ttu-id="13da6-149">Escolha o **associar** botão.</span><span class="sxs-lookup"><span data-stu-id="13da6-149">Choose the **Associate** button.</span></span> <span data-ttu-id="13da6-150">O navegador retorna à página inicial do seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="13da6-150">The browser returns to your application's home page.</span></span>

<a id="To_enable_Facebook_logins"></a>
## <a name="enabling-facebook-logins"></a><span data-ttu-id="13da6-151">Habilitar os logons do Facebook</span><span class="sxs-lookup"><span data-stu-id="13da6-151">Enabling Facebook Logins</span></span>

1. <span data-ttu-id="13da6-152">Vá para o [site de desenvolvedores do Facebook](https://developers.facebook.com/apps) (faça logon se você ainda não esteja conectado).</span><span class="sxs-lookup"><span data-stu-id="13da6-152">Go to the [Facebook developers site](https://developers.facebook.com/apps) (log in if you're not already logged in).</span></span>
2. <span data-ttu-id="13da6-153">Escolha o **criar novo aplicativo** botão e, em seguida, siga os prompts para nomear e criar o novo aplicativo.</span><span class="sxs-lookup"><span data-stu-id="13da6-153">Choose the **Create New App** button, and then follow the prompts to name and create the new application.</span></span>
3. <span data-ttu-id="13da6-154">Na seção **selecione como o seu aplicativo se integrará ao Facebook**, escolha o **site** seção.</span><span class="sxs-lookup"><span data-stu-id="13da6-154">In the section **Select how your app will integrate with Facebook**, choose the **Website** section.</span></span>
4. <span data-ttu-id="13da6-155">Preencha a **URL do Site** campo com a URL do seu site (por exemplo, `http://www.example.com`).</span><span class="sxs-lookup"><span data-stu-id="13da6-155">Fill in the **Site URL** field with the URL of your site (for example, `http://www.example.com`).</span></span> <span data-ttu-id="13da6-156">O **domínio** campo é opcional; você pode usar isso para fornecer autenticação para um domínio inteiro (como *exemplo.com*).</span><span class="sxs-lookup"><span data-stu-id="13da6-156">The **Domain** field is optional; you can use this to provide authentication for an entire domain (such as *example.com*).</span></span> 

    > [!NOTE]
    > <span data-ttu-id="13da6-157">Se você estiver executando um site no computador local com uma URL como `http://localhost:12345` (em que o número é um número de porta local), você pode adicionar esse valor para o **URL do Site** field para testar seu site.</span><span class="sxs-lookup"><span data-stu-id="13da6-157">If you are running a site on your local computer with a URL like `http://localhost:12345` (where the number is a local port number), you can add this value to the **Site URL** field for testing your site.</span></span> <span data-ttu-id="13da6-158">No entanto, o número da porta de suas alterações de site local a qualquer momento, você precisará atualizar o **URL do Site** campo do seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="13da6-158">However, any time the port number of your local site changes, you will need to update the **Site URL** field of your application.</span></span>
5. <span data-ttu-id="13da6-159">Escolha o **salvar alterações** botão.</span><span class="sxs-lookup"><span data-stu-id="13da6-159">Choose the **Save Changes** button.</span></span>
6. <span data-ttu-id="13da6-160">Escolha o **aplicativos** guia novamente e, em seguida, exibir a página inicial do seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="13da6-160">Choose the **Apps** tab again, and then view the start page for your application.</span></span>
7. <span data-ttu-id="13da6-161">Cópia de **ID do aplicativo** e **segredo do aplicativo** valores para seu aplicativo e colá-los em um arquivo de texto temporário.</span><span class="sxs-lookup"><span data-stu-id="13da6-161">Copy the **App ID** and **App Secret** values for your application and paste them into a temporary text file.</span></span> <span data-ttu-id="13da6-162">Você irá passar esses valores para o provedor do Facebook no código do site.</span><span class="sxs-lookup"><span data-stu-id="13da6-162">You will pass these values to the Facebook provider in your website code.</span></span>
8. <span data-ttu-id="13da6-163">Saia do site do desenvolvedor do Facebook.</span><span class="sxs-lookup"><span data-stu-id="13da6-163">Exit the Facebook developer site.</span></span>

<span data-ttu-id="13da6-164">Agora você fazer alterações para duas páginas em seu site para que os usuários serão capazes de fazer logon no site usando suas contas do Facebook.</span><span class="sxs-lookup"><span data-stu-id="13da6-164">Now you make changes to two pages in your website so that users will able to log into the site using their Facebook accounts.</span></span>

1. <span data-ttu-id="13da6-165">Criar ou abrir um site de páginas da Web ASP.NET com base no modelo de Site do WebMatrix inicial.</span><span class="sxs-lookup"><span data-stu-id="13da6-165">Create or open an ASP.NET Web Pages site that's based on the WebMatrix Starter Site template.</span></span>
2. <span data-ttu-id="13da6-166">Abra o  *\_AppStart.cshtml* página e remova o código para o provedor de OAuth do Facebook.</span><span class="sxs-lookup"><span data-stu-id="13da6-166">Open the *\_AppStart.cshtml* page and uncomment the code for the Facebook OAuth provider.</span></span> <span data-ttu-id="13da6-167">O bloco de código sem marca de comentário é semelhante ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="13da6-167">The uncommented code block looks like the following:</span></span> 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample2.cs)]
3. <span data-ttu-id="13da6-168">Cópia de **ID do aplicativo** o valor do aplicativo Facebook como o valor do `appId` parâmetro (entre aspas).</span><span class="sxs-lookup"><span data-stu-id="13da6-168">Copy the **App ID** value from the Facebook application as the value of the `appId` parameter (inside the quotation marks).</span></span>
4. <span data-ttu-id="13da6-169">Cópia **segredo do aplicativo** o valor do aplicativo Facebook como o `appSecret` valor do parâmetro.</span><span class="sxs-lookup"><span data-stu-id="13da6-169">Copy **App Secret** value from the Facebook application as the `appSecret` parameter value.</span></span>
5. <span data-ttu-id="13da6-170">Salve e feche o arquivo.</span><span class="sxs-lookup"><span data-stu-id="13da6-170">Save and close the file.</span></span>

### <a name="testing-facebook-login"></a><span data-ttu-id="13da6-171">Testar o logon do Facebook</span><span class="sxs-lookup"><span data-stu-id="13da6-171">Testing Facebook login</span></span>

1. <span data-ttu-id="13da6-172">Executar o site *default. cshtml* da página e escolher o **Login** botão.</span><span class="sxs-lookup"><span data-stu-id="13da6-172">Run the site's *default.cshtml* page and choose the **Login** button.</span></span>
2. <span data-ttu-id="13da6-173">No *logon* página, o **Use outro serviço para fazer logon** , escolha o **Facebook** ícone.</span><span class="sxs-lookup"><span data-stu-id="13da6-173">On the *Login* page, in the **Use another service to log in** section, choose the **Facebook** icon.</span></span> 

    <span data-ttu-id="13da6-174">A página da web redireciona a solicitação para a página de logon do Facebook.</span><span class="sxs-lookup"><span data-stu-id="13da6-174">The web page redirects the request to the Facebook login page.</span></span>

    ![OAuth 2](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image4.png)
3. <span data-ttu-id="13da6-176">Faça logon em uma conta do Facebook.</span><span class="sxs-lookup"><span data-stu-id="13da6-176">Log into a Facebook account.</span></span> 

    <span data-ttu-id="13da6-177">O código usa o token do Facebook para autenticá-lo e, em seguida, retorna a uma página onde você pode associar seu logon do Facebook com o logon do seu site.</span><span class="sxs-lookup"><span data-stu-id="13da6-177">The code uses the Facebook token to authenticate you and then returns to a page where you can associate your Facebook login with your site's login.</span></span> <span data-ttu-id="13da6-178">Seu endereço de email ou nome de usuário é preenchido para o **Email** campo no formulário.</span><span class="sxs-lookup"><span data-stu-id="13da6-178">Your user name or email address is filled into the **Email** field on the form.</span></span>

    ![OAuth 5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image5.png)
4. <span data-ttu-id="13da6-180">Escolha o **associar** botão.</span><span class="sxs-lookup"><span data-stu-id="13da6-180">Choose the **Associate** button.</span></span> 

    <span data-ttu-id="13da6-181">O navegador retorna à home page e você está conectado.</span><span class="sxs-lookup"><span data-stu-id="13da6-181">The browser returns to the home page and you are logged in.</span></span>

<a id="To_enable_Twitter_logins"></a>
## <a name="enabling-twitter-logins"></a><span data-ttu-id="13da6-182">Habilitar os logons do Twitter</span><span class="sxs-lookup"><span data-stu-id="13da6-182">Enabling Twitter Logins</span></span>

1. <span data-ttu-id="13da6-183">Navegue até a [site de desenvolvedores do Twitter](https://dev.twitter.com/).</span><span class="sxs-lookup"><span data-stu-id="13da6-183">Browse to the [Twitter developers site](https://dev.twitter.com/).</span></span>
2. <span data-ttu-id="13da6-184">Escolha o **criar um aplicativo** vincular e, em seguida, faça logon no site.</span><span class="sxs-lookup"><span data-stu-id="13da6-184">Choose the **Create an App** link and then log into the site.</span></span>
3. <span data-ttu-id="13da6-185">Sobre o **criar um aplicativo** formar, preencha o **nome** e **descrição** campos.</span><span class="sxs-lookup"><span data-stu-id="13da6-185">On the **Create an Application** form, fill in the **Name** and **Description** fields.</span></span>
4. <span data-ttu-id="13da6-186">No **site** , insira a URL do seu site (por exemplo, `http://www.example.com`).</span><span class="sxs-lookup"><span data-stu-id="13da6-186">In the **WebSite** field, enter the URL of your site (for example, `http://www.example.com`).</span></span> 

    > [!NOTE]
    > <span data-ttu-id="13da6-187">Se você estiver testando o seu site localmente (usando uma URL como `http://localhost:12345`), Twitter pode não aceitar a URL.</span><span class="sxs-lookup"><span data-stu-id="13da6-187">If you're testing your site locally (using a URL like `http://localhost:12345`), Twitter might not accept the URL.</span></span> <span data-ttu-id="13da6-188">No entanto, você poderá usar o endereço IP de loopback local (por exemplo `http://127.0.0.1:12345`).</span><span class="sxs-lookup"><span data-stu-id="13da6-188">However, you might be able to use the local loopback IP address (for example `http://127.0.0.1:12345`).</span></span> <span data-ttu-id="13da6-189">Isso simplifica o processo de testar seu aplicativo localmente.</span><span class="sxs-lookup"><span data-stu-id="13da6-189">This simplifies the process of testing your application locally.</span></span> <span data-ttu-id="13da6-190">No entanto, sempre que o número da porta do seu site local for alterada, você precisará atualizar o **site** campo do seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="13da6-190">However, every time the port number of your local site changes, you'll need to update the **WebSite** field of your application.</span></span>
5. <span data-ttu-id="13da6-191">No **URL de retorno de chamada** , insira uma URL para a página em seu site que você deseja que os usuários retornem ao depois de fazer logon no Twitter.</span><span class="sxs-lookup"><span data-stu-id="13da6-191">In the **Callback URL** field, enter a URL for the page in your website that you want users to return to after logging into Twitter.</span></span> <span data-ttu-id="13da6-192">Por exemplo, para enviar aos usuários para a home page do Site inicial (que reconheça seu status conectado), insira a mesma URL que você inseriu na **site** campo.</span><span class="sxs-lookup"><span data-stu-id="13da6-192">For example, to send users to the home page of the Starter Site (which will recognize their logged-in status), enter the same URL that you entered in the **WebSite** field.</span></span>
6. <span data-ttu-id="13da6-193">Aceite os termos e escolha o **criar seu aplicativo Twitter** botão.</span><span class="sxs-lookup"><span data-stu-id="13da6-193">Accept the terms and choose the **Create your Twitter application** button.</span></span>
7. <span data-ttu-id="13da6-194">Sobre o **meus aplicativos** inicial da página, escolha o aplicativo que você criou.</span><span class="sxs-lookup"><span data-stu-id="13da6-194">On the **My Applications** landing page, choose the application you created.</span></span>
8. <span data-ttu-id="13da6-195">Sobre o **detalhes** guia, role para baixo e escolha o **criar meu Token de acesso** botão.</span><span class="sxs-lookup"><span data-stu-id="13da6-195">On the **Details** tab, scroll to the bottom and choose the **Create My Access Token** button.</span></span>
9. <span data-ttu-id="13da6-196">Sobre o **detalhes** guia, copie o **chave do consumidor** e **segredo do consumidor** valores para seu aplicativo e colá-los em um arquivo de texto temporário.</span><span class="sxs-lookup"><span data-stu-id="13da6-196">On the **Details** tab, copy the **Consumer Key** and **Consumer Secret** values for your application and paste them into a temporary text file.</span></span> <span data-ttu-id="13da6-197">Você irá passar esses valores para o provedor do Twitter no código do site.</span><span class="sxs-lookup"><span data-stu-id="13da6-197">You'll pass these values to the Twitter provider in your website code.</span></span>
10. <span data-ttu-id="13da6-198">Saia do site do Twitter.</span><span class="sxs-lookup"><span data-stu-id="13da6-198">Exit the Twitter site.</span></span>

<span data-ttu-id="13da6-199">Agora você alterar duas páginas em seu site para que os usuários poderão fazer logon no site usando suas contas do Twitter.</span><span class="sxs-lookup"><span data-stu-id="13da6-199">Now you make changes to two pages in your website so that users will be able to log into the site using their Twitter accounts.</span></span>

1. <span data-ttu-id="13da6-200">Criar ou abrir um site de páginas da Web ASP.NET com base no modelo de Site do WebMatrix inicial.</span><span class="sxs-lookup"><span data-stu-id="13da6-200">Create or open an ASP.NET Web Pages site that's based on the WebMatrix Starter Site template.</span></span>
2. <span data-ttu-id="13da6-201">Abra o  *\_AppStart.cshtml* página e remova o código para o provedor de OAuth do Twitter.</span><span class="sxs-lookup"><span data-stu-id="13da6-201">Open the *\_AppStart.cshtml* page and uncomment the code for the Twitter OAuth provider.</span></span> <span data-ttu-id="13da6-202">O bloco de código sem marca de comentário tem esta aparência:</span><span class="sxs-lookup"><span data-stu-id="13da6-202">The uncommented code block looks like this:</span></span> 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample3.cs)]
3. <span data-ttu-id="13da6-203">Cópia de **chave do consumidor** o valor do aplicativo Twitter como o valor do `consumerKey` parâmetro (entre aspas).</span><span class="sxs-lookup"><span data-stu-id="13da6-203">Copy the **Consumer Key** value from the Twitter application as the value of the `consumerKey` parameter (inside the quotation marks).</span></span>
4. <span data-ttu-id="13da6-204">Cópia de **segredo do consumidor** o valor do aplicativo Twitter como o valor do `consumerSecret` parâmetro.</span><span class="sxs-lookup"><span data-stu-id="13da6-204">Copy the **Consumer Secret** value from the Twitter application as the value of the `consumerSecret` parameter.</span></span>
5. <span data-ttu-id="13da6-205">Salve e feche o arquivo.</span><span class="sxs-lookup"><span data-stu-id="13da6-205">Save and close the file.</span></span>

### <a name="testing-twitter-login"></a><span data-ttu-id="13da6-206">Testar o logon do Twitter</span><span class="sxs-lookup"><span data-stu-id="13da6-206">Testing Twitter login</span></span>

1. <span data-ttu-id="13da6-207">Execute o *default. cshtml* página do seu site e escolha o **Login** botão.</span><span class="sxs-lookup"><span data-stu-id="13da6-207">Run the *default.cshtml* page of your site and choose the **Login** button.</span></span>
2. <span data-ttu-id="13da6-208">No *logon* página, o **Use outro serviço para fazer logon** , escolha o **Twitter** ícone.</span><span class="sxs-lookup"><span data-stu-id="13da6-208">On the *Login* page, in the **Use another service to log in** section, choose the **Twitter** icon.</span></span> 

    <span data-ttu-id="13da6-209">A página da web redireciona a solicitação para uma página de logon do Twitter para o aplicativo que você criou.</span><span class="sxs-lookup"><span data-stu-id="13da6-209">The web page redirects the request to a Twitter login page for the application you created.</span></span>

    ![OAuth 4](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image6.png)
3. <span data-ttu-id="13da6-211">Faça logon em uma conta do Twitter.</span><span class="sxs-lookup"><span data-stu-id="13da6-211">Log into a Twitter account.</span></span>
4. <span data-ttu-id="13da6-212">O código usa o token do Twitter para autenticar o usuário e, em seguida, retorna a uma página onde você pode associar seu logon com sua conta do site.</span><span class="sxs-lookup"><span data-stu-id="13da6-212">The code uses the Twitter token to authenticate the user and then returns you to a page where you can associate your login with your website account.</span></span> <span data-ttu-id="13da6-213">Seu nome ou endereço de email está preenchido para o **Email** campo no formulário.</span><span class="sxs-lookup"><span data-stu-id="13da6-213">Your name or email address is filled into the **Email** field on the form.</span></span>

    ![OAuth 5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image7.png)
5. <span data-ttu-id="13da6-215">Escolha o **associar** botão.</span><span class="sxs-lookup"><span data-stu-id="13da6-215">Choose the **Associate** button.</span></span> 

    <span data-ttu-id="13da6-216">O navegador retorna à home page e você está conectado.</span><span class="sxs-lookup"><span data-stu-id="13da6-216">The browser returns to the home page and you are logged in.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="13da6-217">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="13da6-217">Additional Resources</span></span>


- [<span data-ttu-id="13da6-218">Personalizar o comportamento de todo o site</span><span class="sxs-lookup"><span data-stu-id="13da6-218">Customizing Site-Wide Behavior</span></span>](https://go.microsoft.com/fwlink/?LinkId=202906)
- [<span data-ttu-id="13da6-219">Adicionar segurança e associação a um Site de páginas da Web do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="13da6-219">Adding Security and Membership to an ASP.NET Web Pages Site</span></span>](https://go.microsoft.com/fwlink/?LinkID=202904)
