---
uid: mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
title: Aplicativo ASP.NET MVC 5 com SMS e o email de autenticação de dois fatores | Microsoft Docs
author: Rick-Anderson
description: Este tutorial mostra como criar um aplicativo da web ASP.NET MVC 5 com a autenticação de dois fatores. Você deve concluir o aplicativo web de criar um seguro ASP.NET MVC 5 com...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/20/2015
ms.topic: article
ms.assetid: f50a5cdb-c06a-46ed-aa14-fc5b049dc8dc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
msc.type: authoredcontent
ms.openlocfilehash: 5e1c54b3901f2c8c85134445c1fa91ee9f2e0d59
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication"></a><span data-ttu-id="17a30-104">Aplicativo ASP.NET MVC 5 com SMS e o email de autenticação de dois fatores</span><span class="sxs-lookup"><span data-stu-id="17a30-104">ASP.NET MVC 5 app with SMS and email Two-Factor Authentication</span></span>
====================
<span data-ttu-id="17a30-105">por [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="17a30-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="17a30-106">Este tutorial mostra como criar um aplicativo da web ASP.NET MVC 5 com a autenticação de dois fatores.</span><span class="sxs-lookup"><span data-stu-id="17a30-106">This tutorial shows you how to build an ASP.NET MVC 5 web app with Two-Factor Authentication.</span></span> <span data-ttu-id="17a30-107">Você deve concluir [criar um aplicativo web seguro do ASP.NET MVC 5 com logon, redefinição de senha e de confirmação de email](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="17a30-107">You should complete [Create a secure ASP.NET MVC 5 web app with log in, email confirmation and password reset](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) before proceeding.</span></span> <span data-ttu-id="17a30-108">Você pode baixar o aplicativo concluído [aqui](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span><span class="sxs-lookup"><span data-stu-id="17a30-108">You can download the completed application [here](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span></span> <span data-ttu-id="17a30-109">O download contém os auxiliares de depuração que permitem que você teste confirmação por email e SMS sem configurar um email ou o provedor de SMS.</span><span class="sxs-lookup"><span data-stu-id="17a30-109">The download contains debugging helpers that let you test email confirmation and SMS without setting up an email or SMS provider.</span></span>
> 
> <span data-ttu-id="17a30-110">Este tutorial foi escrito por [Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).</span><span class="sxs-lookup"><span data-stu-id="17a30-110">This tutorial was written by [Rick Anderson](https://blogs.msdn.com/rickAndy) ( Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).</span></span>


- [<span data-ttu-id="17a30-111">Criar um aplicativo ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="17a30-111">Create an ASP.NET MVC app</span></span>](#createMvc)
- [<span data-ttu-id="17a30-112">Configurar o SMS para autenticação de dois fatores</span><span class="sxs-lookup"><span data-stu-id="17a30-112">Set up SMS for Two-factor authentication</span></span>](#SMS)
- [<span data-ttu-id="17a30-113">Habilitar a autenticação de dois fatores</span><span class="sxs-lookup"><span data-stu-id="17a30-113">Enable Two-factor authentication</span></span>](#enable2)
- [<span data-ttu-id="17a30-114">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="17a30-114">Additional Resources</span></span>](#addRes)

<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a><span data-ttu-id="17a30-115">Criar um aplicativo ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="17a30-115">Create an ASP.NET MVC app</span></span>

<span data-ttu-id="17a30-116">Comece instalando e executando [Visual Studio Express 2013 para Web](https://go.microsoft.com/fwlink/?LinkId=299058) ou [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="17a30-116">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="17a30-117">Instalar [Visual Studio 2013 atualização 3](https://go.microsoft.com/fwlink/?LinkId=390465) ou superior.</span><span class="sxs-lookup"><span data-stu-id="17a30-117">Install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher.</span></span>

> [!NOTE]
> <span data-ttu-id="17a30-118">Aviso: Você deve concluir [criar um aplicativo web seguro do ASP.NET MVC 5 com logon, redefinição de senha e de confirmação de email](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="17a30-118">Warning: You should complete [Create a secure ASP.NET MVC 5 web app with log in, email confirmation and password reset](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) before proceeding.</span></span> <span data-ttu-id="17a30-119">Você deve instalar [Visual Studio 2013 atualização 3](https://go.microsoft.com/fwlink/?LinkId=390465) ou superior para concluir este tutorial.</span><span class="sxs-lookup"><span data-stu-id="17a30-119">You must install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher to complete this tutorial.</span></span>


1. <span data-ttu-id="17a30-120">Criar um novo projeto da Web do ASP.NET e selecione o modelo MVC.</span><span class="sxs-lookup"><span data-stu-id="17a30-120">Create a new ASP.NET Web project and select the MVC template.</span></span> <span data-ttu-id="17a30-121">Formulários da Web também oferece suporte a identidade do ASP.NET, você pode seguir etapas semelhantes em um aplicativo de formulários da web.</span><span class="sxs-lookup"><span data-stu-id="17a30-121">Web Forms also supports ASP.NET Identity, so you could follow similar steps in a web forms app.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image1.png)
2. <span data-ttu-id="17a30-122">Deixe a autenticação padrão como **contas de usuário individuais**.</span><span class="sxs-lookup"><span data-stu-id="17a30-122">Leave the default authentication as **Individual User Accounts**.</span></span> <span data-ttu-id="17a30-123">Se você quiser hospedar o aplicativo no Azure, deixe a caixa de seleção marcada.</span><span class="sxs-lookup"><span data-stu-id="17a30-123">If you'd like to host the app in Azure, leave the check box checked.</span></span> <span data-ttu-id="17a30-124">No tutorial posteriormente, implantará no Azure.</span><span class="sxs-lookup"><span data-stu-id="17a30-124">Later in the tutorial we will deploy to Azure.</span></span> <span data-ttu-id="17a30-125">Você pode [abrir uma conta do Azure gratuitamente](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="17a30-125">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>
3. <span data-ttu-id="17a30-126">Definir o [projeto para usar SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span><span class="sxs-lookup"><span data-stu-id="17a30-126">Set the [project to use SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span></span>

<a id="SMS"></a>
## <a name="set-up-sms-for-two-factor-authentication"></a><span data-ttu-id="17a30-127">Configurar o SMS para autenticação de dois fatores</span><span class="sxs-lookup"><span data-stu-id="17a30-127">Set up SMS for Two-factor authentication</span></span>

<span data-ttu-id="17a30-128">Este tutorial fornece instruções sobre como usar o Twilio ou ASPSMS, mas você pode usar qualquer outro provedor SMS.</span><span class="sxs-lookup"><span data-stu-id="17a30-128">This tutorial provides instructions for using either Twilio or ASPSMS but you can use any other SMS provider.</span></span>

1. <span data-ttu-id="17a30-129">**Criando uma conta de usuário com um provedor de SMS**</span><span class="sxs-lookup"><span data-stu-id="17a30-129">**Creating a User Account with an SMS provider**</span></span>  
  
   <span data-ttu-id="17a30-130">Criar um [Twilio](https://www.twilio.com/try-twilio) ou um [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) conta.</span><span class="sxs-lookup"><span data-stu-id="17a30-130">Create a [Twilio](https://www.twilio.com/try-twilio) or an [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) account.</span></span>
2. <span data-ttu-id="17a30-131">**Instalando pacotes adicionais ou adicionando referências de serviço**</span><span class="sxs-lookup"><span data-stu-id="17a30-131">**Installing additional packages or adding service references**</span></span>  
  
   <span data-ttu-id="17a30-132">Twilio:</span><span class="sxs-lookup"><span data-stu-id="17a30-132">Twilio:</span></span>  
   <span data-ttu-id="17a30-133">No Console do Gerenciador de pacotes, digite o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="17a30-133">In the Package Manager Console, enter the following command:</span></span>  
    `Install-Package Twilio`  
  
   <span data-ttu-id="17a30-134">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="17a30-134">ASPSMS:</span></span>  
   <span data-ttu-id="17a30-135">A referência de serviço a seguir precisa ser adicionado:</span><span class="sxs-lookup"><span data-stu-id="17a30-135">The following service reference needs to be added:</span></span>  
  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image2.png)  
  
   <span data-ttu-id="17a30-136">endereço:</span><span class="sxs-lookup"><span data-stu-id="17a30-136">Address:</span></span>  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
   <span data-ttu-id="17a30-137">Namespace:</span><span class="sxs-lookup"><span data-stu-id="17a30-137">Namespace:</span></span>  
    `ASPSMSX2`
3. <span data-ttu-id="17a30-138">**Descobrir as credenciais do usuário do provedor de SMS**</span><span class="sxs-lookup"><span data-stu-id="17a30-138">**Figuring out SMS Provider User credentials**</span></span>  
  
   <span data-ttu-id="17a30-139">Twilio:</span><span class="sxs-lookup"><span data-stu-id="17a30-139">Twilio:</span></span>  
   <span data-ttu-id="17a30-140">Do **painel** guia da sua conta do Twilio, copie o **SID de conta** e **token de autenticação**.</span><span class="sxs-lookup"><span data-stu-id="17a30-140">From the **Dashboard** tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>  
  
   <span data-ttu-id="17a30-141">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="17a30-141">ASPSMS:</span></span>  
   <span data-ttu-id="17a30-142">De suas configurações de conta, navegue até **chave de usuário confidenciais** e copiá-lo junto com seus Self definido **senha**.</span><span class="sxs-lookup"><span data-stu-id="17a30-142">From your account settings, navigate to **Userkey** and copy it together with your self-defined **Password**.</span></span>  
  
   <span data-ttu-id="17a30-143">Mais tarde armazenaremos esses valores no *Web. config* arquivo nas chaves `"SMSAccountIdentification"` e `"SMSAccountPassword"` .</span><span class="sxs-lookup"><span data-stu-id="17a30-143">We will later store these values in the *web.config* file within the keys `"SMSAccountIdentification"` and `"SMSAccountPassword"` .</span></span>
4. <span data-ttu-id="17a30-144">**Especificando SenderID / originador**</span><span class="sxs-lookup"><span data-stu-id="17a30-144">**Specifying SenderID / Originator**</span></span>  
  
   <span data-ttu-id="17a30-145">Twilio:</span><span class="sxs-lookup"><span data-stu-id="17a30-145">Twilio:</span></span>  
   <span data-ttu-id="17a30-146">Do **números** guia, copie o seu número de telefone do Twilio.</span><span class="sxs-lookup"><span data-stu-id="17a30-146">From the **Numbers** tab, copy your Twilio phone number.</span></span>  
  
   <span data-ttu-id="17a30-147">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="17a30-147">ASPSMS:</span></span>  
   <span data-ttu-id="17a30-148">Dentro de **desbloquear originadores** Menu desbloquear originadores de uma ou mais ou escolha um originador alfanumérico (não é suportado por todas as redes).</span><span class="sxs-lookup"><span data-stu-id="17a30-148">Within the **Unlock Originators** Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span>  
  
   <span data-ttu-id="17a30-149">Mais tarde armazenaremos esse valor no *Web. config* arquivo dentro da chave `"SMSAccountFrom"` .</span><span class="sxs-lookup"><span data-stu-id="17a30-149">We will later store this value in the *web.config* file within the key `"SMSAccountFrom"` .</span></span>
5. <span data-ttu-id="17a30-150">**Transferindo as credenciais do provedor SMS em aplicativo**</span><span class="sxs-lookup"><span data-stu-id="17a30-150">**Transferring SMS provider credentials into app**</span></span>  
  
   <span data-ttu-id="17a30-151">Verifique as credenciais e o número de telefone do remetente disponíveis para o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="17a30-151">Make the credentials and sender phone number available to the app.</span></span> <span data-ttu-id="17a30-152">Para manter as coisas simples armazenaremos esses valores no *Web. config* arquivo.</span><span class="sxs-lookup"><span data-stu-id="17a30-152">To keep things simple we will store these values in the *web.config* file.</span></span> <span data-ttu-id="17a30-153">Quando for implantada no Azure, podemos armazenar os valores de segurança no **configurações do aplicativo** guia Configurar de seção no site da web.</span><span class="sxs-lookup"><span data-stu-id="17a30-153">When we deploy to Azure, we can store the values securely in the **app settings** section on the web site configure tab.</span></span> 

    [!code-xml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample1.xml?highlight=8-10)]

    > [!WARNING]
    > <span data-ttu-id="17a30-154">Segurança - nunca armazenar os dados confidenciais em seu código-fonte.</span><span class="sxs-lookup"><span data-stu-id="17a30-154">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="17a30-155">A conta e as credenciais são adicionadas ao código acima para manter o exemplo simples.</span><span class="sxs-lookup"><span data-stu-id="17a30-155">The account and credentials are added to the code above to keep the sample simple.</span></span> <span data-ttu-id="17a30-156">Consulte [práticas recomendadas para a implantação de senhas e outros dados confidenciais em ASP.NET e o Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span><span class="sxs-lookup"><span data-stu-id="17a30-156">See [Best practices for deploying passwords and other sensitive data to ASP.NET and Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span></span>
6. <span data-ttu-id="17a30-157">**Implementação de transferência de dados para o provedor de SMS**</span><span class="sxs-lookup"><span data-stu-id="17a30-157">**Implementation of data transfer to SMS provider**</span></span>  
  
   <span data-ttu-id="17a30-158">Configurar o `SmsService` classe no *aplicativo\_Start\IdentityConfig.cs* arquivo.</span><span class="sxs-lookup"><span data-stu-id="17a30-158">Configure the `SmsService`  class in the *App\_Start\IdentityConfig.cs* file.</span></span>  
  
   <span data-ttu-id="17a30-159">Dependendo do provedor SMS usado ativar o **Twilio** ou **ASPSMS** seção:</span><span class="sxs-lookup"><span data-stu-id="17a30-159">Depending on the used SMS provider activate either the **Twilio** or the **ASPSMS** section:</span></span> 

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample2.cs)]
7. <span data-ttu-id="17a30-160">Atualização de *Views\Manage\Index.cshtml* exibição Razor: (Observação: apenas não remova os comentários no código existente, use o código abaixo.)</span><span class="sxs-lookup"><span data-stu-id="17a30-160">Update the *Views\Manage\Index.cshtml* Razor view: (note: don't just remove the comments in the exiting code, use the code below.)</span></span>  

    [!code-cshtml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample3.cshtml?highlight=29-66)]
8. <span data-ttu-id="17a30-161">Verifique se o `EnableTwoFactorAuthentication` e `DisableTwoFactorAuthentication` métodos de ação no `ManageController` tem o[[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.118).aspx) atributo:</span><span class="sxs-lookup"><span data-stu-id="17a30-161">Verify the `EnableTwoFactorAuthentication` and `DisableTwoFactorAuthentication` action methods in the `ManageController` have the[[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.118).aspx) attribute:</span></span>  

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample4.cs?highlight=3,16)]
9. <span data-ttu-id="17a30-162">Executar o aplicativo e faça logon usando a conta que você registrou anteriormente.</span><span class="sxs-lookup"><span data-stu-id="17a30-162">Run the app and log in with the account you previously registered.</span></span>
10. <span data-ttu-id="17a30-163">Clique em sua ID de usuário, que ativa o `Index` método de ação `Manage` controlador.</span><span class="sxs-lookup"><span data-stu-id="17a30-163">Click on your User ID, which activates the `Index` action method in `Manage` controller.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image3.png)
11. <span data-ttu-id="17a30-164">Clique em Adicionar.</span><span class="sxs-lookup"><span data-stu-id="17a30-164">Click Add.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image4.png)
12. <span data-ttu-id="17a30-165">O `AddPhoneNumber` método de ação exibe uma caixa de diálogo para inserir um número de telefone que pode receber mensagens SMS.</span><span class="sxs-lookup"><span data-stu-id="17a30-165">The `AddPhoneNumber` action method displays a dialog box to enter a phone number that can receive SMS messages.</span></span>

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample5.cs)]

    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image5.png)
13. <span data-ttu-id="17a30-166">Em alguns segundos, você receberá uma mensagem de texto com o código de verificação.</span><span class="sxs-lookup"><span data-stu-id="17a30-166">In a few seconds you will get a text message with the verification code.</span></span> <span data-ttu-id="17a30-167">Insira-o e pressione **enviar**.</span><span class="sxs-lookup"><span data-stu-id="17a30-167">Enter it and press **Submit**.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image6.png)
14. <span data-ttu-id="17a30-168">O modo de gerenciar mostra que o número de telefone foi adicionado.</span><span class="sxs-lookup"><span data-stu-id="17a30-168">The Manage view shows your phone number was added.</span></span>

<a id="enable2"></a>
## <a name="enable-two-factor-authentication"></a><span data-ttu-id="17a30-169">Habilitar a autenticação de dois fatores</span><span class="sxs-lookup"><span data-stu-id="17a30-169">Enable two-factor authentication</span></span>

<span data-ttu-id="17a30-170">No aplicativo do modelo gerado, você precisa usar a interface do usuário para habilitar a autenticação de dois fatores (2FA).</span><span class="sxs-lookup"><span data-stu-id="17a30-170">In the template generated app, you need to use the UI to enable two-factor authentication (2FA).</span></span> <span data-ttu-id="17a30-171">Para habilitar a 2FA, clique em sua ID de usuário (alias de email) na barra de navegação.</span><span class="sxs-lookup"><span data-stu-id="17a30-171">To enable 2FA, click on your user ID (email alias) in the navigation bar.</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image7.png)

<span data-ttu-id="17a30-172">Clique em Habilitar 2FA.</span><span class="sxs-lookup"><span data-stu-id="17a30-172">Click on enable 2FA.</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image8.png)

<span data-ttu-id="17a30-173">Logoff, em seguida, log novamente.</span><span class="sxs-lookup"><span data-stu-id="17a30-173">Logout, then log back in.</span></span> <span data-ttu-id="17a30-174">Se você tiver habilitado o email (consulte meu [tutorial anterior](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)), você pode selecionar o email para 2FA ou SMS.</span><span class="sxs-lookup"><span data-stu-id="17a30-174">If you've enabled email (see my [previous tutorial](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)), you can select the SMS or email for 2FA.</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image9.png)

<span data-ttu-id="17a30-175">A página de código verificar é exibida, onde você pode inserir o código (de SMS ou email).</span><span class="sxs-lookup"><span data-stu-id="17a30-175">The Verify Code page is displayed where you can enter the code (from SMS or email).</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image10.png)

<span data-ttu-id="17a30-176">Clicar no **lembrar este navegador** caixa de seleção isentará a necessidade de usar 2FA para fazer logon ao usar o navegador e o dispositivo em que você tiver marcado a caixa.</span><span class="sxs-lookup"><span data-stu-id="17a30-176">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log in when using the browser and device where you checked the box.</span></span> <span data-ttu-id="17a30-177">Desde que usuários mal-intencionados não podem acessar seu dispositivo, permitindo que 2FA e clicando no **lembrar este navegador** fornece acesso conveniente de senha um etapa, enquanto ainda retém a proteção forte 2FA para todo o acesso dispositivos não confiáveis.</span><span class="sxs-lookup"><span data-stu-id="17a30-177">As long as malicious users can't gain access to your device, enabling 2FA and clicking on the **Remember this browser** will provide you with convenient one step password access, while still retaining strong 2FA protection for all access from non-trusted devices.</span></span> <span data-ttu-id="17a30-178">Você pode fazer isso em qualquer dispositivo privado que você usa regularmente.</span><span class="sxs-lookup"><span data-stu-id="17a30-178">You can do this on any private device you regularly use.</span></span>

<span data-ttu-id="17a30-179">Este tutorial fornece uma introdução rápida ao habilitar 2FA em um novo aplicativo ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="17a30-179">This tutorial provides a quick introduction to enabling 2FA on a new ASP.NET MVC app.</span></span> <span data-ttu-id="17a30-180">O tutorial [autenticação de dois fatores usando SMS e email com o ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) apresenta detalhes sobre o código de exemplo.</span><span class="sxs-lookup"><span data-stu-id="17a30-180">My tutorial [Two-factor authentication using SMS and email with ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) goes into detail on the code behind the sample.</span></span>

<a id="addRes"></a>
## <a name="additional-resources"></a><span data-ttu-id="17a30-181">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="17a30-181">Additional Resources</span></span>

- <span data-ttu-id="17a30-182">[Autenticação de dois fatores usando SMS e email com o ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) apresenta detalhes sobre a autenticação de dois fatores</span><span class="sxs-lookup"><span data-stu-id="17a30-182">[Two-factor authentication using SMS and email with ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) Goes into detail on Two-factor authentication</span></span>
- [<span data-ttu-id="17a30-183">Links para a identidade do ASP.NET recomendado recursos</span><span class="sxs-lookup"><span data-stu-id="17a30-183">Links to ASP.NET Identity Recommended Resources</span></span>](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- <span data-ttu-id="17a30-184">[Conta de confirmação e de recuperação de senha com a identidade do ASP.NET](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) apresenta mais detalhes na confirmação da senha da conta e a recuperação.</span><span class="sxs-lookup"><span data-stu-id="17a30-184">[Account Confirmation and Password Recovery with ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) Goes into more detail on password recovery and account confirmation.</span></span>
- <span data-ttu-id="17a30-185">[Aplicativo do MVC 5 com o Facebook, Twitter, LinkedIn e logon do Google OAuth2](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) este tutorial mostra como escrever um aplicativo ASP.NET MVC 5 com autorização Facebook e Google OAuth 2.</span><span class="sxs-lookup"><span data-stu-id="17a30-185">[MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) This tutorial shows you how to write an ASP.NET MVC 5 app with Facebook and Google OAuth 2 authorization.</span></span> <span data-ttu-id="17a30-186">Ele também mostra como adicionar dados adicionais para o banco de dados de identidade.</span><span class="sxs-lookup"><span data-stu-id="17a30-186">It also shows how to add additional data to the Identity database.</span></span>
- <span data-ttu-id="17a30-187">[Implantar um aplicativo ASP.NET MVC seguro com associação, OAuth e o banco de dados SQL Web do Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="17a30-187">[Deploy a Secure ASP.NET MVC app with Membership, OAuth, and SQL Database to Azure Web](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span> <span data-ttu-id="17a30-188">Este tutorial adiciona a implantação do Azure, como proteger seu aplicativo com as funções, como usar a API de associação para adicionar usuários e funções e recursos de segurança adicionais.</span><span class="sxs-lookup"><span data-stu-id="17a30-188">This tutorial adds Azure deployment, how to secure your app with roles, how to use the membership API to add users and roles, and additional security features.</span></span>
- [<span data-ttu-id="17a30-189">Criar um aplicativo do Google OAuth 2 e conectar o aplicativo ao projeto</span><span class="sxs-lookup"><span data-stu-id="17a30-189">Creating a Google app for OAuth 2 and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [<span data-ttu-id="17a30-190">Criando o aplicativo no Facebook e conectar o aplicativo ao projeto</span><span class="sxs-lookup"><span data-stu-id="17a30-190">Creating the app in Facebook and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [<span data-ttu-id="17a30-191">Configurar o SSL no projeto</span><span class="sxs-lookup"><span data-stu-id="17a30-191">Setting up SSL in the Project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
