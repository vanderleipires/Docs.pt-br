---
title: "Autenticação de dois fatores com o SMS"
author: rick-anderson
description: "Mostra como configurar a autenticação de dois fatores (2FA) com o ASP.NET Core"
keywords: "ASP.NET Core, SMS, autenticação, 2FA, autenticação de dois fatores, autenticação de dois fatores"
ms.author: riande
manager: wpickett
ms.date: 08/15/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/2fa
ms.openlocfilehash: 05ce53fe9b65f85867a33fdff974b384bb943d37
ms.sourcegitcommit: 67f54fabbfa4e3942f5bfe1f8a7fdfe4a7a75358
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/19/2017
---
# <a name="two-factor-authentication-with-sms"></a><span data-ttu-id="d5e84-104">Autenticação de dois fatores com o SMS</span><span class="sxs-lookup"><span data-stu-id="d5e84-104">Two-factor authentication with SMS</span></span>

<span data-ttu-id="d5e84-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT) e [desenvolvedores Suíça](https://github.com/Swiss-Devs)</span><span class="sxs-lookup"><span data-stu-id="d5e84-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Swiss-Devs](https://github.com/Swiss-Devs)</span></span>

<span data-ttu-id="d5e84-106">Este tutorial aplica-se ao ASP.NET Core apenas 1. x.</span><span class="sxs-lookup"><span data-stu-id="d5e84-106">This tutorial applies to ASP.NET Core 1.x only.</span></span> <span data-ttu-id="d5e84-107">Consulte [geração de código de QR habilitando para aplicativos de autenticador no ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) para ASP.NET Core 2.0 e posterior.</span><span class="sxs-lookup"><span data-stu-id="d5e84-107">See [Enabling QR Code generation for authenticator apps in ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) for ASP.NET Core 2.0 and later.</span></span>

<span data-ttu-id="d5e84-108">Este tutorial mostra como configurar a autenticação de dois fatores (2FA) usando o SMS.</span><span class="sxs-lookup"><span data-stu-id="d5e84-108">This tutorial shows how to set up two-factor authentication (2FA) using SMS.</span></span> <span data-ttu-id="d5e84-109">As instruções são fornecidas para [twilio](https://www.twilio.com/) e [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), mas você pode usar qualquer outro provedor SMS.</span><span class="sxs-lookup"><span data-stu-id="d5e84-109">Instructions are given for [twilio](https://www.twilio.com/) and [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), but you can use any other SMS provider.</span></span> <span data-ttu-id="d5e84-110">Recomendamos que você realize [confirmação de conta e senha de recuperação](accconfirm.md) antes de iniciar este tutorial.</span><span class="sxs-lookup"><span data-stu-id="d5e84-110">We recommend you complete [Account Confirmation and Password Recovery](accconfirm.md) before starting this tutorial.</span></span>

<span data-ttu-id="d5e84-111">Exibição de [exemplo concluído](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span><span class="sxs-lookup"><span data-stu-id="d5e84-111">View the [completed sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span></span> <span data-ttu-id="d5e84-112">[Como baixar](xref:tutorials/index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="d5e84-112">[How to download](xref:tutorials/index#how-to-download-a-sample).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="d5e84-113">Criar um novo projeto ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d5e84-113">Create a new ASP.NET Core project</span></span>

<span data-ttu-id="d5e84-114">Criar um novo aplicativo web de ASP.NET Core denominado `Web2FA` com contas de usuário individuais.</span><span class="sxs-lookup"><span data-stu-id="d5e84-114">Create a new ASP.NET Core web app named `Web2FA` with individual user accounts.</span></span> <span data-ttu-id="d5e84-115">Siga as instruções em [impondo SSL em um aplicativo do ASP.NET Core](xref:security/enforcing-ssl) para configurar e exigir SSL.</span><span class="sxs-lookup"><span data-stu-id="d5e84-115">Follow the instructions in [Enforcing SSL in an ASP.NET Core app](xref:security/enforcing-ssl) to set up and require SSL.</span></span>

### <a name="create-an-sms-account"></a><span data-ttu-id="d5e84-116">Criar uma conta do SMS</span><span class="sxs-lookup"><span data-stu-id="d5e84-116">Create an SMS account</span></span>

<span data-ttu-id="d5e84-117">Criar uma conta SMS, por exemplo, de [twilio](https://www.twilio.com/) ou [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span><span class="sxs-lookup"><span data-stu-id="d5e84-117">Create an SMS account, for example, from [twilio](https://www.twilio.com/) or [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span></span> <span data-ttu-id="d5e84-118">Registre as credenciais de autenticação (para o twilio: accountSid e authToken para ASPSMS: chave de usuário confidenciais e a senha).</span><span class="sxs-lookup"><span data-stu-id="d5e84-118">Record the authentication credentials (for twilio: accountSid and authToken, for ASPSMS: Userkey and Password).</span></span>

#### <a name="figuring-out-sms-provider-credentials"></a><span data-ttu-id="d5e84-119">Descobrir as credenciais do provedor de SMS</span><span class="sxs-lookup"><span data-stu-id="d5e84-119">Figuring out SMS Provider credentials</span></span>

<span data-ttu-id="d5e84-120">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="d5e84-120">**Twilio:**</span></span>  
<span data-ttu-id="d5e84-121">Na guia Painel de sua conta do Twilio, copie o **SID de conta** e **token de autenticação**.</span><span class="sxs-lookup"><span data-stu-id="d5e84-121">From the Dashboard tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>

<span data-ttu-id="d5e84-122">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="d5e84-122">**ASPSMS:**</span></span>  
<span data-ttu-id="d5e84-123">De suas configurações de conta, navegue até **chave de usuário confidenciais** e copie-a junto com seu **senha**.</span><span class="sxs-lookup"><span data-stu-id="d5e84-123">From your account settings, navigate to **Userkey** and copy it together with your **Password**.</span></span>

<span data-ttu-id="d5e84-124">Mais tarde armazenaremos esses valores com a ferramenta de Gerenciador de segredo nas chaves `SMSAccountIdentification` e `SMSAccountPassword`.</span><span class="sxs-lookup"><span data-stu-id="d5e84-124">We will later store these values in with the secret-manager tool within the keys `SMSAccountIdentification` and `SMSAccountPassword`.</span></span>

#### <a name="specifying-senderid--originator"></a><span data-ttu-id="d5e84-125">Especificando SenderID / originador</span><span class="sxs-lookup"><span data-stu-id="d5e84-125">Specifying SenderID / Originator</span></span>

<span data-ttu-id="d5e84-126">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="d5e84-126">**Twilio:**</span></span>  
<span data-ttu-id="d5e84-127">Na guia números, copie o Twilio **número de telefone**.</span><span class="sxs-lookup"><span data-stu-id="d5e84-127">From the Numbers tab, copy your Twilio **phone number**.</span></span> 

<span data-ttu-id="d5e84-128">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="d5e84-128">**ASPSMS:**</span></span>  
<span data-ttu-id="d5e84-129">No Menu originadores desbloquear desbloquear originadores de uma ou mais ou escolha um originador alfanumérico (não é suportado por todas as redes).</span><span class="sxs-lookup"><span data-stu-id="d5e84-129">Within the Unlock Originators Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span> 

<span data-ttu-id="d5e84-130">Posteriormente, armazenará esse valor com a ferramenta Gerenciador de segredo na chave do `SMSAccountFrom`.</span><span class="sxs-lookup"><span data-stu-id="d5e84-130">We will later store this value with the secret-manager tool within the key `SMSAccountFrom`.</span></span>


### <a name="provide-credentials-for-the-sms-service"></a><span data-ttu-id="d5e84-131">Forneça credenciais para o serviço SMS</span><span class="sxs-lookup"><span data-stu-id="d5e84-131">Provide credentials for the SMS service</span></span>

<span data-ttu-id="d5e84-132">Usaremos o [padrão de opções](xref:fundamentals/configuration#options-config-objects) para acessar as configurações de conta e a chave de usuário.</span><span class="sxs-lookup"><span data-stu-id="d5e84-132">We'll use the [Options pattern](xref:fundamentals/configuration#options-config-objects) to access the user account and key settings.</span></span> 

   * <span data-ttu-id="d5e84-133">Crie uma classe para obter a chave segura do SMS.</span><span class="sxs-lookup"><span data-stu-id="d5e84-133">Create a class to fetch the secure SMS key.</span></span> <span data-ttu-id="d5e84-134">Para este exemplo, o `SMSoptions` classe é criada no *Services/SMSoptions.cs* arquivo.</span><span class="sxs-lookup"><span data-stu-id="d5e84-134">For this sample, the `SMSoptions` class is created in the *Services/SMSoptions.cs* file.</span></span>

[!code-csharp[Main](2fa/sample/Web2FA/Services/SMSoptions.cs)]

<span data-ttu-id="d5e84-135">Definir o `SMSAccountIdentification`, `SMSAccountPassword` e `SMSAccountFrom` com o [ferramenta Gerenciador de segredo](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="d5e84-135">Set the `SMSAccountIdentification`, `SMSAccountPassword` and `SMSAccountFrom` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="d5e84-136">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="d5e84-136">For example:</span></span>

```none
C:/Web2FA/src/WebApp1>dotnet user-secrets set SMSAccountIdentification 12345
info: Successfully saved SMSAccountIdentification = 12345 to the secret store.
```
* <span data-ttu-id="d5e84-137">Adicione o pacote NuGet do provedor de SMS.</span><span class="sxs-lookup"><span data-stu-id="d5e84-137">Add the NuGet package for the SMS provider.</span></span> <span data-ttu-id="d5e84-138">Do pacote Manager Console (PMC) executar:</span><span class="sxs-lookup"><span data-stu-id="d5e84-138">From the Package Manager Console (PMC) run:</span></span>

<span data-ttu-id="d5e84-139">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="d5e84-139">**Twilio:**</span></span>  
`Install-Package Twilio`

<span data-ttu-id="d5e84-140">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="d5e84-140">**ASPSMS:**</span></span>  
`Install-Package ASPSMS`


* <span data-ttu-id="d5e84-141">Adicione código de *Services/MessageServices.cs* arquivo para habilitar o SMS.</span><span class="sxs-lookup"><span data-stu-id="d5e84-141">Add code in the *Services/MessageServices.cs* file to enable SMS.</span></span> <span data-ttu-id="d5e84-142">Use o Twilio ou seção ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="d5e84-142">Use either the Twilio or the ASPSMS section:</span></span>


<span data-ttu-id="d5e84-143">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="d5e84-143">**Twilio:**</span></span>  
[!code-csharp[Main](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]

<span data-ttu-id="d5e84-144">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="d5e84-144">**ASPSMS:**</span></span>  
[!code-csharp[Main](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]

### <a name="configure-startup-to-use-smsoptions"></a><span data-ttu-id="d5e84-145">Configurar a inicialização para usar`SMSoptions`</span><span class="sxs-lookup"><span data-stu-id="d5e84-145">Configure startup to use `SMSoptions`</span></span>

<span data-ttu-id="d5e84-146">Adicionar `SMSoptions` no contêiner de serviço no `ConfigureServices` método o *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="d5e84-146">Add `SMSoptions` to the service container in the `ConfigureServices` method in the *Startup.cs*:</span></span>

[!code-csharp[Main](2fa/sample/Web2FA/Startup.cs?name=snippet1&highlight=4)]

### <a name="enable-two-factor-authentication"></a><span data-ttu-id="d5e84-147">Habilitar a autenticação de dois fatores</span><span class="sxs-lookup"><span data-stu-id="d5e84-147">Enable two-factor authentication</span></span>

<span data-ttu-id="d5e84-148">Abra o *Views/Manage/Index.cshtml* o arquivo de exibição Razor e remover caracteres de comentário (portanto, nenhuma marcação é comentário removido).</span><span class="sxs-lookup"><span data-stu-id="d5e84-148">Open the *Views/Manage/Index.cshtml* Razor view file and remove the comment characters (so no markup is commnted out).</span></span>

## <a name="log-in-with-two-factor-authentication"></a><span data-ttu-id="d5e84-149">Faça logon com a autenticação de dois fatores</span><span class="sxs-lookup"><span data-stu-id="d5e84-149">Log in with two-factor authentication</span></span>

* <span data-ttu-id="d5e84-150">Execute o aplicativo e registrar um novo usuário</span><span class="sxs-lookup"><span data-stu-id="d5e84-150">Run the app and register a new user</span></span>

![Exibição de registro de aplicativo da Web aberta no Microsoft Edge](2fa/_static/login2fa1.png)

* <span data-ttu-id="d5e84-152">Toque em seu nome de usuário que ativa o `Index` método de ação no controlador de gerenciamento.</span><span class="sxs-lookup"><span data-stu-id="d5e84-152">Tap on your user name, which activates the `Index` action method in Manage controller.</span></span> <span data-ttu-id="d5e84-153">Em seguida, toque o número de telefone **adicionar** link.</span><span class="sxs-lookup"><span data-stu-id="d5e84-153">Then tap the phone number **Add** link.</span></span>

![Gerenciar o modo de exibição](2fa/_static/login2fa2.png)

* <span data-ttu-id="d5e84-155">Adicionar um número de telefone que receberá o código de verificação e toque em **enviar o código de verificação**.</span><span class="sxs-lookup"><span data-stu-id="d5e84-155">Add a phone number that will receive the verification code, and tap **Send verification code**.</span></span>

![Adicionar página de número de telefone](2fa/_static/login2fa3.png)

* <span data-ttu-id="d5e84-157">Você receberá uma mensagem de texto com o código de verificação.</span><span class="sxs-lookup"><span data-stu-id="d5e84-157">You will get a text message with the verification code.</span></span> <span data-ttu-id="d5e84-158">Insira-o e toque em **enviar**</span><span class="sxs-lookup"><span data-stu-id="d5e84-158">Enter it and tap **Submit**</span></span>

![Verifique se a página de número de telefone](2fa/_static/login2fa4.png)

<span data-ttu-id="d5e84-160">Se você não receber uma mensagem de texto, consulte a página de registro do twilio.</span><span class="sxs-lookup"><span data-stu-id="d5e84-160">If you don't get a text message, see twilio log page.</span></span>

* <span data-ttu-id="d5e84-161">O modo de gerenciar mostra que o número de telefone foi adicionado com êxito.</span><span class="sxs-lookup"><span data-stu-id="d5e84-161">The Manage view shows your phone number was added successfully.</span></span>

![Gerenciar o modo de exibição](2fa/_static/login2fa5.png)

* <span data-ttu-id="d5e84-163">Toque em **habilitar** para habilitar a autenticação de dois fatores.</span><span class="sxs-lookup"><span data-stu-id="d5e84-163">Tap **Enable** to enable two-factor authentication.</span></span>

![Gerenciar o modo de exibição](2fa/_static/login2fa6.png)

### <a name="test-two-factor-authentication"></a><span data-ttu-id="d5e84-165">Autenticação de dois fatores do teste</span><span class="sxs-lookup"><span data-stu-id="d5e84-165">Test two-factor authentication</span></span>

* <span data-ttu-id="d5e84-166">Fazer logoff.</span><span class="sxs-lookup"><span data-stu-id="d5e84-166">Log off.</span></span>

* <span data-ttu-id="d5e84-167">Iniciar sessão.</span><span class="sxs-lookup"><span data-stu-id="d5e84-167">Log in.</span></span>

* <span data-ttu-id="d5e84-168">A conta de usuário habilitou a autenticação de dois fatores, você precisará fornecer o segundo fator de autenticação.</span><span class="sxs-lookup"><span data-stu-id="d5e84-168">The user account has enabled two-factor authentication, so you have to provide the second factor of authentication .</span></span> <span data-ttu-id="d5e84-169">Neste tutorial você ativou a verificação do telefone.</span><span class="sxs-lookup"><span data-stu-id="d5e84-169">In this tutorial you have enabled phone verification.</span></span> <span data-ttu-id="d5e84-170">Os modelos internos de também permitem que você configurar o email como o segundo fator.</span><span class="sxs-lookup"><span data-stu-id="d5e84-170">The built in templates also allow you to set up email as the second factor.</span></span> <span data-ttu-id="d5e84-171">Você pode configurar os fatores adicionais de segundo para autenticação, como códigos QR.</span><span class="sxs-lookup"><span data-stu-id="d5e84-171">You can set up additional second factors for authentication such as QR codes.</span></span> <span data-ttu-id="d5e84-172">Toque em **enviar**.</span><span class="sxs-lookup"><span data-stu-id="d5e84-172">Tap **Submit**.</span></span>

![Enviar o modo de exibição de código de verificação](2fa/_static/login2fa7.png)

* <span data-ttu-id="d5e84-174">Insira o código que você pode obter na mensagem SMS.</span><span class="sxs-lookup"><span data-stu-id="d5e84-174">Enter the code you get in the SMS message.</span></span>

* <span data-ttu-id="d5e84-175">Clicar no **lembrar este navegador** caixa de seleção isentará a necessidade de usar 2FA para fazer logon ao usar o mesmo dispositivo e o navegador.</span><span class="sxs-lookup"><span data-stu-id="d5e84-175">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log on when using the same device and browser.</span></span> <span data-ttu-id="d5e84-176">Habilitando 2FA e clicando em **lembrar este navegador** fornecerá 2FA forte proteção contra usuários mal-intencionados que tentar acessar sua conta, desde que eles não têm acesso ao dispositivo.</span><span class="sxs-lookup"><span data-stu-id="d5e84-176">Enabling 2FA and clicking on **Remember this browser** will provide you with strong 2FA protection from malicious users trying to access your account, as long as they don't have access to your device.</span></span> <span data-ttu-id="d5e84-177">Você pode fazer isso em qualquer dispositivo privado que você usa regularmente.</span><span class="sxs-lookup"><span data-stu-id="d5e84-177">You can do this on any private device you regularly use.</span></span> <span data-ttu-id="d5e84-178">Definindo **lembrar este navegador**, obter a segurança adicional de 2FA de dispositivos que você não usa regularmente e obter a conveniência de não ter percorrer 2FA no seus próprios dispositivos.</span><span class="sxs-lookup"><span data-stu-id="d5e84-178">By setting  **Remember this browser**, you get the added security of 2FA from devices you don't regularly use, and you get the convenience on not having to go through 2FA on your own devices.</span></span>

![Verifique se o modo de exibição](2fa/_static/login2fa8.png)

## <a name="account-lockout-for-protecting-against-brute-force-attacks"></a><span data-ttu-id="d5e84-180">Bloqueio de conta para proteger contra ataques de força bruta</span><span class="sxs-lookup"><span data-stu-id="d5e84-180">Account lockout for protecting against brute force attacks</span></span>

<span data-ttu-id="d5e84-181">Recomendamos que você use o bloqueio de conta com 2FA.</span><span class="sxs-lookup"><span data-stu-id="d5e84-181">We recommend you use account lockout with 2FA.</span></span> <span data-ttu-id="d5e84-182">Depois que um usuário fizer logon (por meio de uma conta local ou social), cada tentativa falha de 2FA é armazenada e se o máximo de tentativas (o padrão é 5) for atingido, o usuário é bloqueado por cinco minutos (você pode definir o bloqueio de tempo com `DefaultAccountLockoutTimeSpan`).</span><span class="sxs-lookup"><span data-stu-id="d5e84-182">Once a user logs in (through a local account or social account), each failed attempt at 2FA is stored, and if the maximum attempts (default is 5) is reached, the user is locked out for five minutes (you can set the lock out time with `DefaultAccountLockoutTimeSpan`).</span></span> <span data-ttu-id="d5e84-183">A seguir configura a conta seja bloqueada por 10 minutos após 10 tentativas com falha.</span><span class="sxs-lookup"><span data-stu-id="d5e84-183">The following configures Account to be locked out for 10 minutes after 10 failed attempts.</span></span>

[!code-csharp[Main](2fa/sample/Web2FA/Startup.cs?name=snippet2&highlight=13-17)] 
