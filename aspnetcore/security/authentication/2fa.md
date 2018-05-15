---
title: Autenticação de dois fatores com SMS no núcleo do ASP.NET
author: rick-anderson
description: Saiba como configurar a autenticação de dois fatores (2FA) com um aplicativo do ASP.NET Core.
manager: wpickett
monikerRange: < aspnetcore-2.0
ms.author: riande
ms.date: 08/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/2fa
ms.openlocfilehash: 20f00c2307e140d81e716304c96a143340d934d0
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/08/2018
---
# <a name="two-factor-authentication-with-sms-in-aspnet-core"></a><span data-ttu-id="07a5a-103">Autenticação de dois fatores com SMS no núcleo do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="07a5a-103">Two-factor authentication with SMS in ASP.NET Core</span></span>

<span data-ttu-id="07a5a-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT) e [desenvolvedores Suíça](https://github.com/Swiss-Devs)</span><span class="sxs-lookup"><span data-stu-id="07a5a-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Swiss-Devs](https://github.com/Swiss-Devs)</span></span>

<span data-ttu-id="07a5a-105">Consulte [geração de código de QR habilitar para aplicativos de autenticador no ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) para ASP.NET Core 2.0 e posterior.</span><span class="sxs-lookup"><span data-stu-id="07a5a-105">See [Enable QR Code generation for authenticator apps in ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) for ASP.NET Core 2.0 and later.</span></span>

<span data-ttu-id="07a5a-106">Este tutorial mostra como configurar a autenticação de dois fatores (2FA) usando o SMS.</span><span class="sxs-lookup"><span data-stu-id="07a5a-106">This tutorial shows how to set up two-factor authentication (2FA) using SMS.</span></span> <span data-ttu-id="07a5a-107">As instruções são fornecidas para [twilio](https://www.twilio.com/) e [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), mas você pode usar qualquer outro provedor SMS.</span><span class="sxs-lookup"><span data-stu-id="07a5a-107">Instructions are given for [twilio](https://www.twilio.com/) and [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), but you can use any other SMS provider.</span></span> <span data-ttu-id="07a5a-108">Recomendamos que você realize [confirmação de conta e senha de recuperação](xref:security/authentication/accconfirm) antes de iniciar este tutorial.</span><span class="sxs-lookup"><span data-stu-id="07a5a-108">We recommend you complete [Account Confirmation and Password Recovery](xref:security/authentication/accconfirm) before starting this tutorial.</span></span>

<span data-ttu-id="07a5a-109">Exibição de [exemplo concluído](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span><span class="sxs-lookup"><span data-stu-id="07a5a-109">View the [completed sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span></span> <span data-ttu-id="07a5a-110">[Como baixar](xref:tutorials/index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="07a5a-110">[How to download](xref:tutorials/index#how-to-download-a-sample).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="07a5a-111">Criar um novo projeto ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="07a5a-111">Create a new ASP.NET Core project</span></span>

<span data-ttu-id="07a5a-112">Criar um novo aplicativo web de ASP.NET Core denominado `Web2FA` com contas de usuário individuais.</span><span class="sxs-lookup"><span data-stu-id="07a5a-112">Create a new ASP.NET Core web app named `Web2FA` with individual user accounts.</span></span> <span data-ttu-id="07a5a-113">Siga as instruções em [impor SSL em um aplicativo do ASP.NET Core](xref:security/enforcing-ssl) para configurar e exigir SSL.</span><span class="sxs-lookup"><span data-stu-id="07a5a-113">Follow the instructions in [Enforce SSL in an ASP.NET Core app](xref:security/enforcing-ssl) to set up and require SSL.</span></span>

### <a name="create-an-sms-account"></a><span data-ttu-id="07a5a-114">Criar uma conta do SMS</span><span class="sxs-lookup"><span data-stu-id="07a5a-114">Create an SMS account</span></span>

<span data-ttu-id="07a5a-115">Criar uma conta SMS, por exemplo, de [twilio](https://www.twilio.com/) ou [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span><span class="sxs-lookup"><span data-stu-id="07a5a-115">Create an SMS account, for example, from [twilio](https://www.twilio.com/) or [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span></span> <span data-ttu-id="07a5a-116">Registre as credenciais de autenticação (para o twilio: accountSid e authToken para ASPSMS: chave de usuário confidenciais e a senha).</span><span class="sxs-lookup"><span data-stu-id="07a5a-116">Record the authentication credentials (for twilio: accountSid and authToken, for ASPSMS: Userkey and Password).</span></span>

#### <a name="figuring-out-sms-provider-credentials"></a><span data-ttu-id="07a5a-117">Descobrir as credenciais do provedor de SMS</span><span class="sxs-lookup"><span data-stu-id="07a5a-117">Figuring out SMS Provider credentials</span></span>

<span data-ttu-id="07a5a-118">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="07a5a-118">**Twilio:**</span></span>  
<span data-ttu-id="07a5a-119">Na guia Painel de sua conta do Twilio, copie o **SID de conta** e **token de autenticação**.</span><span class="sxs-lookup"><span data-stu-id="07a5a-119">From the Dashboard tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>

<span data-ttu-id="07a5a-120">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="07a5a-120">**ASPSMS:**</span></span>  
<span data-ttu-id="07a5a-121">De suas configurações de conta, navegue até **chave de usuário confidenciais** e copie-a junto com seu **senha**.</span><span class="sxs-lookup"><span data-stu-id="07a5a-121">From your account settings, navigate to **Userkey** and copy it together with your **Password**.</span></span>

<span data-ttu-id="07a5a-122">Mais tarde armazenaremos esses valores com a ferramenta de Gerenciador de segredo nas chaves `SMSAccountIdentification` e `SMSAccountPassword`.</span><span class="sxs-lookup"><span data-stu-id="07a5a-122">We will later store these values in with the secret-manager tool within the keys `SMSAccountIdentification` and `SMSAccountPassword`.</span></span>

#### <a name="specifying-senderid--originator"></a><span data-ttu-id="07a5a-123">Especificando SenderID / originador</span><span class="sxs-lookup"><span data-stu-id="07a5a-123">Specifying SenderID / Originator</span></span>

<span data-ttu-id="07a5a-124">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="07a5a-124">**Twilio:**</span></span>  
<span data-ttu-id="07a5a-125">Na guia números, copie o Twilio **número de telefone**.</span><span class="sxs-lookup"><span data-stu-id="07a5a-125">From the Numbers tab, copy your Twilio **phone number**.</span></span> 

<span data-ttu-id="07a5a-126">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="07a5a-126">**ASPSMS:**</span></span>  
<span data-ttu-id="07a5a-127">No Menu originadores desbloquear desbloquear originadores de uma ou mais ou escolha um originador alfanumérico (não é suportado por todas as redes).</span><span class="sxs-lookup"><span data-stu-id="07a5a-127">Within the Unlock Originators Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span> 

<span data-ttu-id="07a5a-128">Posteriormente, armazenará esse valor com a ferramenta Gerenciador de segredo na chave do `SMSAccountFrom`.</span><span class="sxs-lookup"><span data-stu-id="07a5a-128">We will later store this value with the secret-manager tool within the key `SMSAccountFrom`.</span></span>


### <a name="provide-credentials-for-the-sms-service"></a><span data-ttu-id="07a5a-129">Forneça credenciais para o serviço SMS</span><span class="sxs-lookup"><span data-stu-id="07a5a-129">Provide credentials for the SMS service</span></span>

<span data-ttu-id="07a5a-130">Usaremos o [padrão de opções](xref:fundamentals/configuration/options) para acessar as configurações de conta e a chave de usuário.</span><span class="sxs-lookup"><span data-stu-id="07a5a-130">We'll use the [Options pattern](xref:fundamentals/configuration/options) to access the user account and key settings.</span></span> 

   * <span data-ttu-id="07a5a-131">Crie uma classe para obter a chave segura do SMS.</span><span class="sxs-lookup"><span data-stu-id="07a5a-131">Create a class to fetch the secure SMS key.</span></span> <span data-ttu-id="07a5a-132">Para este exemplo, o `SMSoptions` classe é criada no *Services/SMSoptions.cs* arquivo.</span><span class="sxs-lookup"><span data-stu-id="07a5a-132">For this sample, the `SMSoptions` class is created in the *Services/SMSoptions.cs* file.</span></span>

[!code-csharp[](2fa/sample/Web2FA/Services/SMSoptions.cs)]

<span data-ttu-id="07a5a-133">Definir o `SMSAccountIdentification`, `SMSAccountPassword` e `SMSAccountFrom` com o [ferramenta Gerenciador de segredo](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="07a5a-133">Set the `SMSAccountIdentification`, `SMSAccountPassword` and `SMSAccountFrom` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="07a5a-134">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="07a5a-134">For example:</span></span>

```none
C:/Web2FA/src/WebApp1>dotnet user-secrets set SMSAccountIdentification 12345
info: Successfully saved SMSAccountIdentification = 12345 to the secret store.
```
* <span data-ttu-id="07a5a-135">Adicione o pacote NuGet do provedor de SMS.</span><span class="sxs-lookup"><span data-stu-id="07a5a-135">Add the NuGet package for the SMS provider.</span></span> <span data-ttu-id="07a5a-136">Do pacote Manager Console (PMC) executar:</span><span class="sxs-lookup"><span data-stu-id="07a5a-136">From the Package Manager Console (PMC) run:</span></span>

<span data-ttu-id="07a5a-137">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="07a5a-137">**Twilio:**</span></span>  
`Install-Package Twilio`

<span data-ttu-id="07a5a-138">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="07a5a-138">**ASPSMS:**</span></span>  
`Install-Package ASPSMS`


* <span data-ttu-id="07a5a-139">Adicione código de *Services/MessageServices.cs* arquivo para habilitar o SMS.</span><span class="sxs-lookup"><span data-stu-id="07a5a-139">Add code in the *Services/MessageServices.cs* file to enable SMS.</span></span> <span data-ttu-id="07a5a-140">Use o Twilio ou seção ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="07a5a-140">Use either the Twilio or the ASPSMS section:</span></span>


<span data-ttu-id="07a5a-141">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="07a5a-141">**Twilio:**</span></span>  
[!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]

<span data-ttu-id="07a5a-142">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="07a5a-142">**ASPSMS:**</span></span>  
[!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]

### <a name="configure-startup-to-use-smsoptions"></a><span data-ttu-id="07a5a-143">Configurar a inicialização para usar `SMSoptions`</span><span class="sxs-lookup"><span data-stu-id="07a5a-143">Configure startup to use `SMSoptions`</span></span>

<span data-ttu-id="07a5a-144">Adicionar `SMSoptions` no contêiner de serviço no `ConfigureServices` método o *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="07a5a-144">Add `SMSoptions` to the service container in the `ConfigureServices` method in the *Startup.cs*:</span></span>

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet1&highlight=4)]

### <a name="enable-two-factor-authentication"></a><span data-ttu-id="07a5a-145">Habilitar a autenticação de dois fatores</span><span class="sxs-lookup"><span data-stu-id="07a5a-145">Enable two-factor authentication</span></span>

<span data-ttu-id="07a5a-146">Abra o *Views/Manage/Index.cshtml* o arquivo de exibição Razor e remover caracteres de comentário (portanto, nenhuma marcação é comentário removido).</span><span class="sxs-lookup"><span data-stu-id="07a5a-146">Open the *Views/Manage/Index.cshtml* Razor view file and remove the comment characters (so no markup is commnted out).</span></span>

## <a name="log-in-with-two-factor-authentication"></a><span data-ttu-id="07a5a-147">Faça logon com a autenticação de dois fatores</span><span class="sxs-lookup"><span data-stu-id="07a5a-147">Log in with two-factor authentication</span></span>

* <span data-ttu-id="07a5a-148">Execute o aplicativo e registrar um novo usuário</span><span class="sxs-lookup"><span data-stu-id="07a5a-148">Run the app and register a new user</span></span>

![Exibição de registro de aplicativo da Web aberta no Microsoft Edge](2fa/_static/login2fa1.png)

* <span data-ttu-id="07a5a-150">Toque em seu nome de usuário que ativa o `Index` método de ação no controlador de gerenciamento.</span><span class="sxs-lookup"><span data-stu-id="07a5a-150">Tap on your user name, which activates the `Index` action method in Manage controller.</span></span> <span data-ttu-id="07a5a-151">Em seguida, toque o número de telefone **adicionar** link.</span><span class="sxs-lookup"><span data-stu-id="07a5a-151">Then tap the phone number **Add** link.</span></span>

![Gerenciar o modo de exibição](2fa/_static/login2fa2.png)

* <span data-ttu-id="07a5a-153">Adicionar um número de telefone que receberá o código de verificação e toque em **enviar o código de verificação**.</span><span class="sxs-lookup"><span data-stu-id="07a5a-153">Add a phone number that will receive the verification code, and tap **Send verification code**.</span></span>

![Adicionar página de número de telefone](2fa/_static/login2fa3.png)

* <span data-ttu-id="07a5a-155">Você receberá uma mensagem de texto com o código de verificação.</span><span class="sxs-lookup"><span data-stu-id="07a5a-155">You will get a text message with the verification code.</span></span> <span data-ttu-id="07a5a-156">Insira-o e toque em **enviar**</span><span class="sxs-lookup"><span data-stu-id="07a5a-156">Enter it and tap **Submit**</span></span>

![Verifique se a página de número de telefone](2fa/_static/login2fa4.png)

<span data-ttu-id="07a5a-158">Se você não receber uma mensagem de texto, consulte a página de registro do twilio.</span><span class="sxs-lookup"><span data-stu-id="07a5a-158">If you don't get a text message, see twilio log page.</span></span>

* <span data-ttu-id="07a5a-159">O modo de gerenciar mostra que o número de telefone foi adicionado com êxito.</span><span class="sxs-lookup"><span data-stu-id="07a5a-159">The Manage view shows your phone number was added successfully.</span></span>

![Gerenciar o modo de exibição](2fa/_static/login2fa5.png)

* <span data-ttu-id="07a5a-161">Toque em **habilitar** para habilitar a autenticação de dois fatores.</span><span class="sxs-lookup"><span data-stu-id="07a5a-161">Tap **Enable** to enable two-factor authentication.</span></span>

![Gerenciar o modo de exibição](2fa/_static/login2fa6.png)

### <a name="test-two-factor-authentication"></a><span data-ttu-id="07a5a-163">Autenticação de dois fatores do teste</span><span class="sxs-lookup"><span data-stu-id="07a5a-163">Test two-factor authentication</span></span>

* <span data-ttu-id="07a5a-164">Fazer logoff.</span><span class="sxs-lookup"><span data-stu-id="07a5a-164">Log off.</span></span>

* <span data-ttu-id="07a5a-165">Iniciar sessão.</span><span class="sxs-lookup"><span data-stu-id="07a5a-165">Log in.</span></span>

* <span data-ttu-id="07a5a-166">A conta de usuário habilitou a autenticação de dois fatores, você precisará fornecer o segundo fator de autenticação.</span><span class="sxs-lookup"><span data-stu-id="07a5a-166">The user account has enabled two-factor authentication, so you have to provide the second factor of authentication .</span></span> <span data-ttu-id="07a5a-167">Neste tutorial você ativou a verificação do telefone.</span><span class="sxs-lookup"><span data-stu-id="07a5a-167">In this tutorial you have enabled phone verification.</span></span> <span data-ttu-id="07a5a-168">Os modelos internos de também permitem que você configurar o email como o segundo fator.</span><span class="sxs-lookup"><span data-stu-id="07a5a-168">The built in templates also allow you to set up email as the second factor.</span></span> <span data-ttu-id="07a5a-169">Você pode configurar os fatores adicionais de segundo para autenticação, como códigos QR.</span><span class="sxs-lookup"><span data-stu-id="07a5a-169">You can set up additional second factors for authentication such as QR codes.</span></span> <span data-ttu-id="07a5a-170">Toque em **enviar**.</span><span class="sxs-lookup"><span data-stu-id="07a5a-170">Tap **Submit**.</span></span>

![Enviar o modo de exibição de código de verificação](2fa/_static/login2fa7.png)

* <span data-ttu-id="07a5a-172">Insira o código que você pode obter na mensagem SMS.</span><span class="sxs-lookup"><span data-stu-id="07a5a-172">Enter the code you get in the SMS message.</span></span>

* <span data-ttu-id="07a5a-173">Clicar no **lembrar este navegador** caixa de seleção isentará a necessidade de usar 2FA para fazer logon ao usar o mesmo dispositivo e o navegador.</span><span class="sxs-lookup"><span data-stu-id="07a5a-173">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log on when using the same device and browser.</span></span> <span data-ttu-id="07a5a-174">Habilitando 2FA e clicando em **lembrar este navegador** fornecerá 2FA forte proteção contra usuários mal-intencionados que tentar acessar sua conta, desde que eles não têm acesso ao dispositivo.</span><span class="sxs-lookup"><span data-stu-id="07a5a-174">Enabling 2FA and clicking on **Remember this browser** will provide you with strong 2FA protection from malicious users trying to access your account, as long as they don't have access to your device.</span></span> <span data-ttu-id="07a5a-175">Você pode fazer isso em qualquer dispositivo privado que você usa regularmente.</span><span class="sxs-lookup"><span data-stu-id="07a5a-175">You can do this on any private device you regularly use.</span></span> <span data-ttu-id="07a5a-176">Definindo **lembrar este navegador**, obter a segurança adicional de 2FA de dispositivos que você não usa regularmente e obter a conveniência de não ter percorrer 2FA no seus próprios dispositivos.</span><span class="sxs-lookup"><span data-stu-id="07a5a-176">By setting  **Remember this browser**, you get the added security of 2FA from devices you don't regularly use, and you get the convenience on not having to go through 2FA on your own devices.</span></span>

![Verifique se o modo de exibição](2fa/_static/login2fa8.png)

## <a name="account-lockout-for-protecting-against-brute-force-attacks"></a><span data-ttu-id="07a5a-178">Bloqueio de conta para proteger contra ataques de força bruta</span><span class="sxs-lookup"><span data-stu-id="07a5a-178">Account lockout for protecting against brute force attacks</span></span>

<span data-ttu-id="07a5a-179">Bloqueio de conta é recomendado com 2FA.</span><span class="sxs-lookup"><span data-stu-id="07a5a-179">Account lockout is recommended with 2FA.</span></span> <span data-ttu-id="07a5a-180">Depois que um usuário faz logon por meio de uma conta local ou sociais, cada tentativa falha de 2FA é armazenada.</span><span class="sxs-lookup"><span data-stu-id="07a5a-180">Once a user signs in through a local account or social account, each failed attempt at 2FA is stored.</span></span> <span data-ttu-id="07a5a-181">Se as tentativas de acesso com falha máximo for atingido, o usuário está bloqueado (padrão: bloqueio de 5 minutos após 5 tentativas de acesso).</span><span class="sxs-lookup"><span data-stu-id="07a5a-181">If the maximum failed access attempts is reached, the user is locked out (default: 5 minute lockout after 5 failed access attempts).</span></span> <span data-ttu-id="07a5a-182">Uma autenticação bem-sucedida redefine a contagem de tentativas de acesso com falha e redefine o relógio.</span><span class="sxs-lookup"><span data-stu-id="07a5a-182">A successful authentication resets the failed access attempts count and resets the clock.</span></span> <span data-ttu-id="07a5a-183">O máximo de tentativas de acesso com falha e tempo de bloqueio pode ser definido com [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) e [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan).</span><span class="sxs-lookup"><span data-stu-id="07a5a-183">The maximum failed access attempts and lockout time can be set with [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) and [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan).</span></span> <span data-ttu-id="07a5a-184">A seguir configura o bloqueio de conta para 10 minutos após 10 tentativas de acesso:</span><span class="sxs-lookup"><span data-stu-id="07a5a-184">The following configures account lockout for 10 minutes after 10 failed access attempts:</span></span>

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet2&highlight=13-17)]

<span data-ttu-id="07a5a-185">Confirme se [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync) define `lockoutOnFailure` para `true`:</span><span class="sxs-lookup"><span data-stu-id="07a5a-185">Confirm that [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync) sets `lockoutOnFailure` to `true`:</span></span>

```csharp
var result = await _signInManager.PasswordSignInAsync(
                 Input.Email, Input.Password, Input.RememberMe, lockoutOnFailure: true);
```
