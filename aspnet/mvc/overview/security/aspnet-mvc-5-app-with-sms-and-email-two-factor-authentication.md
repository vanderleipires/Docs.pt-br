---
uid: mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
title: Aplicativo ASP.NET MVC 5 com o SMS e email de autenticação de dois fatores | Microsoft Docs
author: Rick-Anderson
description: Este tutorial mostra como criar um aplicativo de web do ASP.NET MVC 5 com autenticação de dois fatores. Você deve concluir o aplicativo web de criar um ASP.NET MVC 5 seguro com...
ms.author: riande
ms.date: 08/20/2015
ms.assetid: f50a5cdb-c06a-46ed-aa14-fc5b049dc8dc
msc.legacyurl: /mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
msc.type: authoredcontent
ms.openlocfilehash: a422988f681273b153725d32bb8337deb25b12f1
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912586"
---
<a name="aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication"></a><span data-ttu-id="d5ffe-104">Aplicativo ASP.NET MVC 5 com o SMS e email de autenticação de dois fatores</span><span class="sxs-lookup"><span data-stu-id="d5ffe-104">ASP.NET MVC 5 app with SMS and email Two-Factor Authentication</span></span>
====================
<span data-ttu-id="d5ffe-105">por [Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="d5ffe-105">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

> <span data-ttu-id="d5ffe-106">Este tutorial mostra como criar um aplicativo de web do ASP.NET MVC 5 com autenticação de dois fatores.</span><span class="sxs-lookup"><span data-stu-id="d5ffe-106">This tutorial shows you how to build an ASP.NET MVC 5 web app with Two-Factor Authentication.</span></span> <span data-ttu-id="d5ffe-107">Você deve concluir [criar um aplicativo de web do ASP.NET MVC 5 seguro com logon, redefinição de senha e de confirmação de email](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="d5ffe-107">You should complete [Create a secure ASP.NET MVC 5 web app with log in, email confirmation and password reset](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) before proceeding.</span></span> <span data-ttu-id="d5ffe-108">Você pode baixar o aplicativo concluído [aqui](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span><span class="sxs-lookup"><span data-stu-id="d5ffe-108">You can download the completed application [here](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span></span> <span data-ttu-id="d5ffe-109">O download contém os auxiliares de depuração que permitem que você teste confirmação por email e SMS sem configurar um email ou um provedor de SMS.</span><span class="sxs-lookup"><span data-stu-id="d5ffe-109">The download contains debugging helpers that let you test email confirmation and SMS without setting up an email or SMS provider.</span></span>
> 
> <span data-ttu-id="d5ffe-110">Este tutorial foi escrito por [Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).</span><span class="sxs-lookup"><span data-stu-id="d5ffe-110">This tutorial was written by [Rick Anderson](https://blogs.msdn.com/rickAndy) ( Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).</span></span>


- [<span data-ttu-id="d5ffe-111">Criar um aplicativo ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="d5ffe-111">Create an ASP.NET MVC app</span></span>](#createMvc)
- [<span data-ttu-id="d5ffe-112">Configurar o SMS para autenticação de dois fatores</span><span class="sxs-lookup"><span data-stu-id="d5ffe-112">Set up SMS for Two-factor authentication</span></span>](#SMS)
- [<span data-ttu-id="d5ffe-113">Habilitar a autenticação de dois fatores</span><span class="sxs-lookup"><span data-stu-id="d5ffe-113">Enable Two-factor authentication</span></span>](#enable2)
- [<span data-ttu-id="d5ffe-114">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="d5ffe-114">Additional Resources</span></span>](#addRes)

<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a><span data-ttu-id="d5ffe-115">Criar um aplicativo ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="d5ffe-115">Create an ASP.NET MVC app</span></span>

<span data-ttu-id="d5ffe-116">Comece instalando e executando [Visual Studio Express 2013 para Web](https://go.microsoft.com/fwlink/?LinkId=299058) ou [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="d5ffe-116">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="d5ffe-117">Instale [Visual Studio 2013 atualização 3](https://go.microsoft.com/fwlink/?LinkId=390465) ou superior.</span><span class="sxs-lookup"><span data-stu-id="d5ffe-117">Install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher.</span></span>

> [!NOTE]
> <span data-ttu-id="d5ffe-118">Aviso: Você deve concluir [criar um aplicativo de web do ASP.NET MVC 5 seguro com logon, redefinição de senha e de confirmação de email](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="d5ffe-118">Warning: You should complete [Create a secure ASP.NET MVC 5 web app with log in, email confirmation and password reset](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) before proceeding.</span></span> <span data-ttu-id="d5ffe-119">Você deve instalar [Visual Studio 2013 atualização 3](https://go.microsoft.com/fwlink/?LinkId=390465) ou superior para concluir este tutorial.</span><span class="sxs-lookup"><span data-stu-id="d5ffe-119">You must install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher to complete this tutorial.</span></span>


1. <span data-ttu-id="d5ffe-120">Crie um novo projeto da Web do ASP.NET e selecione o modelo MVC.</span><span class="sxs-lookup"><span data-stu-id="d5ffe-120">Create a new ASP.NET Web project and select the MVC template.</span></span> <span data-ttu-id="d5ffe-121">Web Forms também oferece suporte a identidade do ASP.NET, você pode seguir etapas semelhantes em um aplicativo de formulários da web.</span><span class="sxs-lookup"><span data-stu-id="d5ffe-121">Web Forms also supports ASP.NET Identity, so you could follow similar steps in a web forms app.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image1.png)
2. <span data-ttu-id="d5ffe-122">Deixe a autenticação padrão como **contas de usuário individuais**.</span><span class="sxs-lookup"><span data-stu-id="d5ffe-122">Leave the default authentication as **Individual User Accounts**.</span></span> <span data-ttu-id="d5ffe-123">Se você quiser hospedar o aplicativo no Azure, deixe a caixa de seleção marcada.</span><span class="sxs-lookup"><span data-stu-id="d5ffe-123">If you'd like to host the app in Azure, leave the check box checked.</span></span> <span data-ttu-id="d5ffe-124">Posteriormente no tutorial, implantaremos para o Azure.</span><span class="sxs-lookup"><span data-stu-id="d5ffe-124">Later in the tutorial we will deploy to Azure.</span></span> <span data-ttu-id="d5ffe-125">Você pode [abrir uma conta do Azure gratuitamente](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="d5ffe-125">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>
3. <span data-ttu-id="d5ffe-126">Defina as [projeto para usar SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span><span class="sxs-lookup"><span data-stu-id="d5ffe-126">Set the [project to use SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span></span>

<a id="SMS"></a>
## <a name="set-up-sms-for-two-factor-authentication"></a><span data-ttu-id="d5ffe-127">Configurar o SMS para autenticação de dois fatores</span><span class="sxs-lookup"><span data-stu-id="d5ffe-127">Set up SMS for Two-factor authentication</span></span>

<span data-ttu-id="d5ffe-128">Este tutorial fornece instruções sobre como usar Twilio ou ASPSMS, mas você pode usar qualquer outro provedor SMS.</span><span class="sxs-lookup"><span data-stu-id="d5ffe-128">This tutorial provides instructions for using either Twilio or ASPSMS but you can use any other SMS provider.</span></span>

1. <span data-ttu-id="d5ffe-129">**Criando uma conta de usuário com um provedor de SMS**</span><span class="sxs-lookup"><span data-stu-id="d5ffe-129">**Creating a User Account with an SMS provider**</span></span>  
  
   <span data-ttu-id="d5ffe-130">Criar uma [Twilio](https://www.twilio.com/try-twilio) ou um [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) conta.</span><span class="sxs-lookup"><span data-stu-id="d5ffe-130">Create a [Twilio](https://www.twilio.com/try-twilio) or an [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) account.</span></span>
2. <span data-ttu-id="d5ffe-131">**Instalar pacotes adicionais ou adicionar referências de serviço**</span><span class="sxs-lookup"><span data-stu-id="d5ffe-131">**Installing additional packages or adding service references**</span></span>  
  
   <span data-ttu-id="d5ffe-132">Twilio:</span><span class="sxs-lookup"><span data-stu-id="d5ffe-132">Twilio:</span></span>  
   <span data-ttu-id="d5ffe-133">No Console do Gerenciador de pacotes, digite o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="d5ffe-133">In the Package Manager Console, enter the following command:</span></span>  
    `Install-Package Twilio`  
  
   <span data-ttu-id="d5ffe-134">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="d5ffe-134">ASPSMS:</span></span>  
   <span data-ttu-id="d5ffe-135">A seguinte referência de serviço precisa ser adicionado:</span><span class="sxs-lookup"><span data-stu-id="d5ffe-135">The following service reference needs to be added:</span></span>  
  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image2.png)  
  
   <span data-ttu-id="d5ffe-136">Endereço:</span><span class="sxs-lookup"><span data-stu-id="d5ffe-136">Address:</span></span>  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
   <span data-ttu-id="d5ffe-137">Namespace:</span><span class="sxs-lookup"><span data-stu-id="d5ffe-137">Namespace:</span></span>  
    `ASPSMSX2`
3. <span data-ttu-id="d5ffe-138">**Descobrir as credenciais do usuário do provedor de SMS**</span><span class="sxs-lookup"><span data-stu-id="d5ffe-138">**Figuring out SMS Provider User credentials**</span></span>  
  
   <span data-ttu-id="d5ffe-139">Twilio:</span><span class="sxs-lookup"><span data-stu-id="d5ffe-139">Twilio:</span></span>  
   <span data-ttu-id="d5ffe-140">Do **Dashboard** guia da sua conta do Twilio, copie o **SID de conta** e **token de autenticação**.</span><span class="sxs-lookup"><span data-stu-id="d5ffe-140">From the **Dashboard** tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>  
  
   <span data-ttu-id="d5ffe-141">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="d5ffe-141">ASPSMS:</span></span>  
   <span data-ttu-id="d5ffe-142">Em suas configurações de conta, navegue até **Userkey** e copie-o junto com seu autodefinido **senha**.</span><span class="sxs-lookup"><span data-stu-id="d5ffe-142">From your account settings, navigate to **Userkey** and copy it together with your self-defined **Password**.</span></span>  
  
   <span data-ttu-id="d5ffe-143">Posteriormente, armazenaremos esses valores na *Web. config* arquivo nas chaves `"SMSAccountIdentification"` e `"SMSAccountPassword"` .</span><span class="sxs-lookup"><span data-stu-id="d5ffe-143">We will later store these values in the *web.config* file within the keys `"SMSAccountIdentification"` and `"SMSAccountPassword"` .</span></span>
4. <span data-ttu-id="d5ffe-144">**Especificando SenderID / originador**</span><span class="sxs-lookup"><span data-stu-id="d5ffe-144">**Specifying SenderID / Originator**</span></span>  
  
   <span data-ttu-id="d5ffe-145">Twilio:</span><span class="sxs-lookup"><span data-stu-id="d5ffe-145">Twilio:</span></span>  
   <span data-ttu-id="d5ffe-146">Dos **números** guia, copie o número de telefone do Twilio.</span><span class="sxs-lookup"><span data-stu-id="d5ffe-146">From the **Numbers** tab, copy your Twilio phone number.</span></span>  
  
   <span data-ttu-id="d5ffe-147">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="d5ffe-147">ASPSMS:</span></span>  
   <span data-ttu-id="d5ffe-148">Dentro de **desbloquear originadores** Menu, desbloquear originadores de um ou mais, ou escolha um originador alfanumérico (não é suportado por todas as redes).</span><span class="sxs-lookup"><span data-stu-id="d5ffe-148">Within the **Unlock Originators** Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span>  
  
   <span data-ttu-id="d5ffe-149">Posteriormente, armazenaremos esse valor na *Web. config* arquivo dentro da chave `"SMSAccountFrom"` .</span><span class="sxs-lookup"><span data-stu-id="d5ffe-149">We will later store this value in the *web.config* file within the key `"SMSAccountFrom"` .</span></span>
5. <span data-ttu-id="d5ffe-150">**Transferir as credenciais do provedor SMS para o aplicativo**</span><span class="sxs-lookup"><span data-stu-id="d5ffe-150">**Transferring SMS provider credentials into app**</span></span>  
  
   <span data-ttu-id="d5ffe-151">Disponibilize as credenciais e o número de telefone do remetente para o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="d5ffe-151">Make the credentials and sender phone number available to the app.</span></span> <span data-ttu-id="d5ffe-152">Para manter as coisas simples, armazenaremos esses valores na *Web. config* arquivo.</span><span class="sxs-lookup"><span data-stu-id="d5ffe-152">To keep things simple we will store these values in the *web.config* file.</span></span> <span data-ttu-id="d5ffe-153">Ao implantar no Azure, podemos armazenar os valores de segurança na **configurações do aplicativo** seção no site da web na guia Configurar.</span><span class="sxs-lookup"><span data-stu-id="d5ffe-153">When we deploy to Azure, we can store the values securely in the **app settings** section on the web site configure tab.</span></span> 

    [!code-xml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample1.xml?highlight=8-10)]

    > [!WARNING]
    > <span data-ttu-id="d5ffe-154">Security - nunca armazenar os dados confidenciais em seu código-fonte.</span><span class="sxs-lookup"><span data-stu-id="d5ffe-154">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="d5ffe-155">A conta e as credenciais são adicionadas ao código acima para manter o exemplo simples.</span><span class="sxs-lookup"><span data-stu-id="d5ffe-155">The account and credentials are added to the code above to keep the sample simple.</span></span> <span data-ttu-id="d5ffe-156">Ver [práticas recomendadas para implantar senhas e outros dados confidenciais no ASP.NET e o Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span><span class="sxs-lookup"><span data-stu-id="d5ffe-156">See [Best practices for deploying passwords and other sensitive data to ASP.NET and Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span></span>
6. <span data-ttu-id="d5ffe-157">**Implementação de transferência de dados para o provedor de SMS**</span><span class="sxs-lookup"><span data-stu-id="d5ffe-157">**Implementation of data transfer to SMS provider**</span></span>  
  
   <span data-ttu-id="d5ffe-158">Configurar o `SmsService` classe os *App\_Start\IdentityConfig.cs* arquivo.</span><span class="sxs-lookup"><span data-stu-id="d5ffe-158">Configure the `SmsService`  class in the *App\_Start\IdentityConfig.cs* file.</span></span>  
  
   <span data-ttu-id="d5ffe-159">Dependendo do provedor de SMS usado ativar o **Twilio** ou o **ASPSMS** seção:</span><span class="sxs-lookup"><span data-stu-id="d5ffe-159">Depending on the used SMS provider activate either the **Twilio** or the **ASPSMS** section:</span></span> 

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample2.cs)]
7. <span data-ttu-id="d5ffe-160">Atualizar o *Views\Manage\Index.cshtml* exibição do Razor: (Observação: apenas não remova os comentários no código existente, use o código a seguir.)</span><span class="sxs-lookup"><span data-stu-id="d5ffe-160">Update the *Views\Manage\Index.cshtml* Razor view: (note: don't just remove the comments in the exiting code, use the code below.)</span></span>  

    [!code-cshtml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample3.cshtml?highlight=29-66)]
8. <span data-ttu-id="d5ffe-161">Verifique se o `EnableTwoFactorAuthentication` e `DisableTwoFactorAuthentication` métodos de ação na `ManageController` têm o[[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.118).aspx) atributo:</span><span class="sxs-lookup"><span data-stu-id="d5ffe-161">Verify the `EnableTwoFactorAuthentication` and `DisableTwoFactorAuthentication` action methods in the `ManageController` have the[[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.118).aspx) attribute:</span></span>  

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample4.cs?highlight=3,16)]
9. <span data-ttu-id="d5ffe-162">Execute o aplicativo e faça logon com a conta que você registrou anteriormente.</span><span class="sxs-lookup"><span data-stu-id="d5ffe-162">Run the app and log in with the account you previously registered.</span></span>
10. <span data-ttu-id="d5ffe-163">Clique em sua ID de usuário, que ativa o `Index` método de ação `Manage` controlador.</span><span class="sxs-lookup"><span data-stu-id="d5ffe-163">Click on your User ID, which activates the `Index` action method in `Manage` controller.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image3.png)
11. <span data-ttu-id="d5ffe-164">Clique em Adicionar.</span><span class="sxs-lookup"><span data-stu-id="d5ffe-164">Click Add.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image4.png)
12. <span data-ttu-id="d5ffe-165">O `AddPhoneNumber` método de ação exibe uma caixa de diálogo para inserir um número de telefone que possa receber mensagens SMS.</span><span class="sxs-lookup"><span data-stu-id="d5ffe-165">The `AddPhoneNumber` action method displays a dialog box to enter a phone number that can receive SMS messages.</span></span>

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample5.cs)]

    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image5.png)
13. <span data-ttu-id="d5ffe-166">Em poucos segundos, você receberá uma mensagem de texto com o código de verificação.</span><span class="sxs-lookup"><span data-stu-id="d5ffe-166">In a few seconds you will get a text message with the verification code.</span></span> <span data-ttu-id="d5ffe-167">Insira-o e pressione **enviar**.</span><span class="sxs-lookup"><span data-stu-id="d5ffe-167">Enter it and press **Submit**.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image6.png)
14. <span data-ttu-id="d5ffe-168">O modo de gerenciar mostra que o número de telefone foi adicionado.</span><span class="sxs-lookup"><span data-stu-id="d5ffe-168">The Manage view shows your phone number was added.</span></span>

<a id="enable2"></a>
## <a name="enable-two-factor-authentication"></a><span data-ttu-id="d5ffe-169">Habilitar a autenticação de dois fatores</span><span class="sxs-lookup"><span data-stu-id="d5ffe-169">Enable two-factor authentication</span></span>

<span data-ttu-id="d5ffe-170">No aplicativo do modelo gerado, você precisará usar a interface do usuário para habilitar a autenticação de dois fatores (2FA).</span><span class="sxs-lookup"><span data-stu-id="d5ffe-170">In the template generated app, you need to use the UI to enable two-factor authentication (2FA).</span></span> <span data-ttu-id="d5ffe-171">Para habilitar a 2FA, clique em sua ID de usuário (alias de email) na barra de navegação.</span><span class="sxs-lookup"><span data-stu-id="d5ffe-171">To enable 2FA, click on your user ID (email alias) in the navigation bar.</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image7.png)

<span data-ttu-id="d5ffe-172">Clique em Habilitar 2FA.</span><span class="sxs-lookup"><span data-stu-id="d5ffe-172">Click on enable 2FA.</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image8.png)

<span data-ttu-id="d5ffe-173">Logoff, em seguida, log novamente.</span><span class="sxs-lookup"><span data-stu-id="d5ffe-173">Logout, then log back in.</span></span> <span data-ttu-id="d5ffe-174">Se você tiver habilitado o email (consulte minha [tutorial anterior](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)), você pode selecionar o SMS ou email para 2FA.</span><span class="sxs-lookup"><span data-stu-id="d5ffe-174">If you've enabled email (see my [previous tutorial](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)), you can select the SMS or email for 2FA.</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image9.png)

<span data-ttu-id="d5ffe-175">A página de código de verificar é exibida, onde você pode inserir o código (de SMS ou email).</span><span class="sxs-lookup"><span data-stu-id="d5ffe-175">The Verify Code page is displayed where you can enter the code (from SMS or email).</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image10.png)

<span data-ttu-id="d5ffe-176">Clicar na **lembrar deste navegador** caixa de seleção serão isentos da necessidade de usar o 2FA para fazer logon ao usar o navegador e o dispositivo em que você tiver marcado a caixa.</span><span class="sxs-lookup"><span data-stu-id="d5ffe-176">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log in when using the browser and device where you checked the box.</span></span> <span data-ttu-id="d5ffe-177">Desde que usuários mal-intencionados não podem obter acesso ao dispositivo, permitindo 2FA e clicando na **lembrar deste navegador** lhe fornecerá acesso conveniente de senha uma única etapa, enquanto ainda retém 2FA forte proteção para todos os acessos em dispositivos não confiáveis.</span><span class="sxs-lookup"><span data-stu-id="d5ffe-177">As long as malicious users can't gain access to your device, enabling 2FA and clicking on the **Remember this browser** will provide you with convenient one step password access, while still retaining strong 2FA protection for all access from non-trusted devices.</span></span> <span data-ttu-id="d5ffe-178">Você pode fazer isso em qualquer dispositivo privado que você usa regularmente.</span><span class="sxs-lookup"><span data-stu-id="d5ffe-178">You can do this on any private device you regularly use.</span></span>

<span data-ttu-id="d5ffe-179">Este tutorial fornece uma introdução rápida ao habilitar a 2FA em um novo aplicativo ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="d5ffe-179">This tutorial provides a quick introduction to enabling 2FA on a new ASP.NET MVC app.</span></span> <span data-ttu-id="d5ffe-180">Tutorial de minha [autenticação de dois fatores usando SMS e email com o ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) apresenta detalhes sobre o código por trás do exemplo.</span><span class="sxs-lookup"><span data-stu-id="d5ffe-180">My tutorial [Two-factor authentication using SMS and email with ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) goes into detail on the code behind the sample.</span></span>

<a id="addRes"></a>
## <a name="additional-resources"></a><span data-ttu-id="d5ffe-181">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="d5ffe-181">Additional Resources</span></span>

- <span data-ttu-id="d5ffe-182">[Autenticação de dois fatores usando SMS e email com o ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) entra em detalhes sobre a autenticação de dois fatores</span><span class="sxs-lookup"><span data-stu-id="d5ffe-182">[Two-factor authentication using SMS and email with ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) Goes into detail on Two-factor authentication</span></span>
- [<span data-ttu-id="d5ffe-183">Recursos recomendados de links para a identidade do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="d5ffe-183">Links to ASP.NET Identity Recommended Resources</span></span>](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- <span data-ttu-id="d5ffe-184">[Confirmação de conta e recuperação de senha com o ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) apresenta mais detalhes na confirmação de conta e recuperação de senha.</span><span class="sxs-lookup"><span data-stu-id="d5ffe-184">[Account Confirmation and Password Recovery with ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) Goes into more detail on password recovery and account confirmation.</span></span>
- <span data-ttu-id="d5ffe-185">[Aplicativo do MVC 5 com o Facebook, Twitter, LinkedIn e Google OAuth2 Sign-on](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) este tutorial mostra como escrever um aplicativo ASP.NET MVC 5 com o Facebook e Google OAuth 2 autorização.</span><span class="sxs-lookup"><span data-stu-id="d5ffe-185">[MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) This tutorial shows you how to write an ASP.NET MVC 5 app with Facebook and Google OAuth 2 authorization.</span></span> <span data-ttu-id="d5ffe-186">Ele também mostra como adicionar dados adicionais para o banco de dados de identidade.</span><span class="sxs-lookup"><span data-stu-id="d5ffe-186">It also shows how to add additional data to the Identity database.</span></span>
- <span data-ttu-id="d5ffe-187">[Implantar um aplicativo ASP.NET MVC seguro com associação, OAuth e banco de dados SQL do Azure Web](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="d5ffe-187">[Deploy a Secure ASP.NET MVC app with Membership, OAuth, and SQL Database to Azure Web](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span> <span data-ttu-id="d5ffe-188">Este tutorial adiciona a implantação do Azure, como proteger seu aplicativo com as funções, como usar a API de associação para adicionar usuários e funções e recursos de segurança adicionais.</span><span class="sxs-lookup"><span data-stu-id="d5ffe-188">This tutorial adds Azure deployment, how to secure your app with roles, how to use the membership API to add users and roles, and additional security features.</span></span>
- [<span data-ttu-id="d5ffe-189">Criando um aplicativo do Google para o OAuth 2 e conectar o aplicativo ao projeto</span><span class="sxs-lookup"><span data-stu-id="d5ffe-189">Creating a Google app for OAuth 2 and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [<span data-ttu-id="d5ffe-190">Criando o aplicativo no Facebook e conectar o aplicativo ao projeto</span><span class="sxs-lookup"><span data-stu-id="d5ffe-190">Creating the app in Facebook and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [<span data-ttu-id="d5ffe-191">Configurar o SSL no projeto</span><span class="sxs-lookup"><span data-stu-id="d5ffe-191">Setting up SSL in the Project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
- [<span data-ttu-id="d5ffe-192">Como configurar seu ambiente de desenvolvimento em C# e ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="d5ffe-192">How to set up your C# and ASP.NET MVC development environment</span></span>](https://www.twilio.com/docs/usage/tutorials/how-to-set-up-your-csharp-and-asp-net-mvc-development-environment)
