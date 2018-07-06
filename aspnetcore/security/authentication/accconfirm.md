---
title: Confirmação de conta e de recuperação de senha no ASP.NET Core
author: rick-anderson
description: Saiba como criar um aplicativo ASP.NET Core com a redefinição de senha e de confirmação de email.
ms.author: riande
ms.date: 2/11/2018
uid: security/authentication/accconfirm
ms.openlocfilehash: 12265903f60ff6d62befc445434db025c244c178
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37803266"
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a><span data-ttu-id="52b07-103">Confirmação de conta e de recuperação de senha no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="52b07-103">Account confirmation and password recovery in ASP.NET Core</span></span>

<span data-ttu-id="52b07-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT) e [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="52b07-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="52b07-105">Este tutorial mostra como criar um aplicativo ASP.NET Core com a redefinição de senha e de confirmação de email.</span><span class="sxs-lookup"><span data-stu-id="52b07-105">This tutorial shows you how to build an ASP.NET Core app with email confirmation and password reset.</span></span> <span data-ttu-id="52b07-106">Este tutorial é **não** um tópico de início.</span><span class="sxs-lookup"><span data-stu-id="52b07-106">This tutorial is **not** a beginning topic.</span></span> <span data-ttu-id="52b07-107">Você deve estar familiarizado com:</span><span class="sxs-lookup"><span data-stu-id="52b07-107">You should be familiar with:</span></span>

* [<span data-ttu-id="52b07-108">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="52b07-108">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="52b07-109">Autenticação</span><span class="sxs-lookup"><span data-stu-id="52b07-109">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="52b07-110">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="52b07-110">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<span data-ttu-id="52b07-111">Ver [esse arquivo PDF](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) para as versões 1.1 do ASP.NET Core MVC e 2. x.</span><span class="sxs-lookup"><span data-stu-id="52b07-111">See [this PDF file](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC 1.1 and 2.x versions.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="52b07-112">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="52b07-112">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs.md)]

## <a name="create-a-new-aspnet-core-project-with-the-net-core-cli"></a><span data-ttu-id="52b07-113">Criar um novo projeto ASP.NET Core com a CLI do .NET Core</span><span class="sxs-lookup"><span data-stu-id="52b07-113">Create a new ASP.NET Core project with the .NET Core CLI</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="52b07-114">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="52b07-114">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new webapp --auth Individual -o WebPWrecover
cd WebPWrecover
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new razor --auth Individual -o WebPWrecover
cd WebPWrecover
```

::: moniker-end

* <span data-ttu-id="52b07-115">`--auth Individual` Especifica o modelo de projeto de contas de usuário individuais.</span><span class="sxs-lookup"><span data-stu-id="52b07-115">`--auth Individual` specifies the Individual User Accounts project template.</span></span>
* <span data-ttu-id="52b07-116">No Windows, adicione o `-uld` opção.</span><span class="sxs-lookup"><span data-stu-id="52b07-116">On Windows, add the `-uld` option.</span></span> <span data-ttu-id="52b07-117">Ele especifica que LocalDB deve ser usado em vez do SQLite.</span><span class="sxs-lookup"><span data-stu-id="52b07-117">It specifies LocalDB should be used instead of SQLite.</span></span>
* <span data-ttu-id="52b07-118">Executar `new mvc --help` para obter ajuda sobre este comando.</span><span class="sxs-lookup"><span data-stu-id="52b07-118">Run `new mvc --help` to get help on this command.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="52b07-119">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="52b07-119">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="52b07-120">Se você estiver usando a CLI ou o SQLite, execute o seguinte em uma janela de comando:</span><span class="sxs-lookup"><span data-stu-id="52b07-120">If you're using the CLI or SQLite, run the following in a command window:</span></span>

```console
dotnet new mvc --auth Individual
```

* <span data-ttu-id="52b07-121">`--auth Individual` Especifica o modelo de projeto de contas de usuário individuais.</span><span class="sxs-lookup"><span data-stu-id="52b07-121">`--auth Individual` specifies the Individual User Accounts project template.</span></span>
* <span data-ttu-id="52b07-122">No Windows, adicione o `-uld` opção.</span><span class="sxs-lookup"><span data-stu-id="52b07-122">On Windows, add the `-uld` option.</span></span> <span data-ttu-id="52b07-123">Ele especifica que LocalDB deve ser usado em vez do SQLite.</span><span class="sxs-lookup"><span data-stu-id="52b07-123">It specifies LocalDB should be used instead of SQLite.</span></span>
* <span data-ttu-id="52b07-124">Executar `new mvc --help` para obter ajuda sobre este comando.</span><span class="sxs-lookup"><span data-stu-id="52b07-124">Run `new mvc --help` to get help on this command.</span></span>

---

<span data-ttu-id="52b07-125">Como alternativa, você pode criar um novo projeto ASP.NET Core com o Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="52b07-125">Alternatively, you can create a new ASP.NET Core project with Visual Studio:</span></span>

* <span data-ttu-id="52b07-126">No Visual Studio, crie um novo **aplicativo Web** projeto.</span><span class="sxs-lookup"><span data-stu-id="52b07-126">In Visual Studio, create a new **Web Application** project.</span></span>
* <span data-ttu-id="52b07-127">Selecione **ASP.NET Core 2.0**.</span><span class="sxs-lookup"><span data-stu-id="52b07-127">Select **ASP.NET Core 2.0**.</span></span> <span data-ttu-id="52b07-128">**.NET core** está selecionado na imagem a seguir, mas você pode selecionar **.NET Framework**.</span><span class="sxs-lookup"><span data-stu-id="52b07-128">**.NET Core** is selected in the following image, but you can select **.NET Framework**.</span></span>
* <span data-ttu-id="52b07-129">Selecione **alterar autenticação** e definido como **contas de usuário individuais**.</span><span class="sxs-lookup"><span data-stu-id="52b07-129">Select **Change Authentication** and set to **Individual User Accounts**.</span></span>
* <span data-ttu-id="52b07-130">Mantenha o padrão **no aplicativo de contas de usuário do Store**.</span><span class="sxs-lookup"><span data-stu-id="52b07-130">Keep the default **Store user accounts in-app**.</span></span>

![Nova caixa de diálogo do projeto mostrando "Opção de contas de usuário individuais" selecionada](accconfirm/_static/2.png)

## <a name="test-new-user-registration"></a><span data-ttu-id="52b07-132">Novo registro de usuário de teste</span><span class="sxs-lookup"><span data-stu-id="52b07-132">Test new user registration</span></span>

<span data-ttu-id="52b07-133">Execute o aplicativo, selecione a **registrar** vincular e registrar um usuário.</span><span class="sxs-lookup"><span data-stu-id="52b07-133">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="52b07-134">Siga as instruções para executar migrações do Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="52b07-134">Follow the instructions to run Entity Framework Core migrations.</span></span> <span data-ttu-id="52b07-135">Neste ponto, a validação apenas no email é com o [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) atributo.</span><span class="sxs-lookup"><span data-stu-id="52b07-135">At this point, the only validation on the email is with the [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attribute.</span></span> <span data-ttu-id="52b07-136">Depois de enviar o registro, você está conectado ao aplicativo.</span><span class="sxs-lookup"><span data-stu-id="52b07-136">After submitting the registration, you are logged into the app.</span></span> <span data-ttu-id="52b07-137">Posteriormente no tutorial, o código é atualizado para que novos usuários não podem fazer logon até que seu email foi validado.</span><span class="sxs-lookup"><span data-stu-id="52b07-137">Later in the tutorial, the code is updated so new users can't log in until their email has been validated.</span></span>

## <a name="view-the-identity-database"></a><span data-ttu-id="52b07-138">Exibir o banco de dados de identidade</span><span class="sxs-lookup"><span data-stu-id="52b07-138">View the Identity database</span></span>

<span data-ttu-id="52b07-139">Ver [trabalhar com SQLite em um projeto do ASP.NET Core MVC](xref:tutorials/first-mvc-app-xplat/working-with-sql) para obter instruções sobre como exibir o banco de dados SQLite.</span><span class="sxs-lookup"><span data-stu-id="52b07-139">See [Work with SQLite in an ASP.NET Core MVC project](xref:tutorials/first-mvc-app-xplat/working-with-sql) for instructions on how to view the SQLite database.</span></span>

<span data-ttu-id="52b07-140">Para o Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="52b07-140">For Visual Studio:</span></span>

* <span data-ttu-id="52b07-141">Dos **modo de exibição** menu, selecione **Pesquisador de objetos do SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="52b07-141">From the **View** menu, select **SQL Server Object Explorer** (SSOX).</span></span>
* <span data-ttu-id="52b07-142">Navegue até **(localdb) MSSQLLocalDB (SQL Server 13)**.</span><span class="sxs-lookup"><span data-stu-id="52b07-142">Navigate to **(localdb)MSSQLLocalDB(SQL Server 13)**.</span></span> <span data-ttu-id="52b07-143">Clique duas vezes em **dbo. AspNetUsers** > **exibir dados**:</span><span class="sxs-lookup"><span data-stu-id="52b07-143">Right-click on **dbo.AspNetUsers** > **View Data**:</span></span>

![Menu contextual em tabela AspNetUsers no Pesquisador de objetos do SQL Server](accconfirm/_static/ssox.png)

<span data-ttu-id="52b07-145">Observe a tabela `EmailConfirmed` campo é `False`.</span><span class="sxs-lookup"><span data-stu-id="52b07-145">Note the table's `EmailConfirmed` field is `False`.</span></span>

<span data-ttu-id="52b07-146">Você talvez queira usar este email novamente na próxima etapa quando o aplicativo envia um email de confirmação.</span><span class="sxs-lookup"><span data-stu-id="52b07-146">You might want to use this email again in the next step when the app sends a confirmation email.</span></span> <span data-ttu-id="52b07-147">Clique com botão direito na linha e selecione **excluir**.</span><span class="sxs-lookup"><span data-stu-id="52b07-147">Right-click on the row and select **Delete**.</span></span> <span data-ttu-id="52b07-148">Excluir o alias de email torna mais fácil nas etapas a seguir.</span><span class="sxs-lookup"><span data-stu-id="52b07-148">Deleting the email alias makes it easier in the following steps.</span></span>

---

## <a name="require-https"></a><span data-ttu-id="52b07-149">Exigir HTTPS</span><span class="sxs-lookup"><span data-stu-id="52b07-149">Require HTTPS</span></span>

<span data-ttu-id="52b07-150">Ver [exigir HTTPS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="52b07-150">See [Require HTTPS](xref:security/enforcing-ssl).</span></span>

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a><span data-ttu-id="52b07-151">Exigir email de confirmação</span><span class="sxs-lookup"><span data-stu-id="52b07-151">Require email confirmation</span></span>

<span data-ttu-id="52b07-152">É uma prática recomendada para confirmar o email de um novo registro de usuário.</span><span class="sxs-lookup"><span data-stu-id="52b07-152">It's a best practice to confirm the email of a new user registration.</span></span> <span data-ttu-id="52b07-153">Ajuda a confirmação para verificar se eles não estiver representando alguém de email (ou seja, eles ainda não registrados com o email de outra pessoa).</span><span class="sxs-lookup"><span data-stu-id="52b07-153">Email confirmation helps to verify they're not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="52b07-154">Suponha que você tinha um fórum de discussão e quisesse impedir "yli@example.com"de registrar-se como"nolivetto@contoso.com".</span><span class="sxs-lookup"><span data-stu-id="52b07-154">Suppose you had a discussion forum, and you wanted to prevent "yli@example.com" from registering as "nolivetto@contoso.com".</span></span> <span data-ttu-id="52b07-155">Sem email de confirmação "nolivetto@contoso.com" pode receber emails indesejados de seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="52b07-155">Without email confirmation, "nolivetto@contoso.com" could receive unwanted email from your app.</span></span> <span data-ttu-id="52b07-156">Suponha que o usuário registrado acidentalmente como "ylo@example.com" e ainda não tenha notado o erro de ortografia de "yli".</span><span class="sxs-lookup"><span data-stu-id="52b07-156">Suppose the user accidentally registered as "ylo@example.com" and hadn't noticed the misspelling of "yli".</span></span> <span data-ttu-id="52b07-157">Eles não seria capazes de usar a recuperação de senha porque o aplicativo não tiver seu email correto.</span><span class="sxs-lookup"><span data-stu-id="52b07-157">They wouldn't be able to use password recovery because the app doesn't have their correct email.</span></span> <span data-ttu-id="52b07-158">Email de confirmação fornece apenas proteção limitada de bots.</span><span class="sxs-lookup"><span data-stu-id="52b07-158">Email confirmation provides only limited protection from bots.</span></span> <span data-ttu-id="52b07-159">Email de confirmação não fornece proteção contra usuários mal-intencionados com muitas contas de email.</span><span class="sxs-lookup"><span data-stu-id="52b07-159">Email confirmation doesn't provide protection from malicious users with many email accounts.</span></span>

<span data-ttu-id="52b07-160">Geralmente você deseja impedir que novos usuários incluam dados em seu site até que eles tenham um email confirmado.</span><span class="sxs-lookup"><span data-stu-id="52b07-160">You generally want to prevent new users from posting any data to your web site before they have a confirmed email.</span></span>

<span data-ttu-id="52b07-161">Atualização `ConfigureServices` para exigir um email confirmado:</span><span class="sxs-lookup"><span data-stu-id="52b07-161">Update `ConfigureServices` to require a confirmed email:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet1&highlight=12-17)]

<span data-ttu-id="52b07-162">`config.SignIn.RequireConfirmedEmail = true;` impede que os usuários registrados entrar até que o email seja confirmado.</span><span class="sxs-lookup"><span data-stu-id="52b07-162">`config.SignIn.RequireConfirmedEmail = true;` prevents registered users from logging in until their email is confirmed.</span></span>

### <a name="configure-email-provider"></a><span data-ttu-id="52b07-163">Configurar o provedor de email</span><span class="sxs-lookup"><span data-stu-id="52b07-163">Configure email provider</span></span>

<span data-ttu-id="52b07-164">Neste tutorial, o SendGrid é usado para enviar email.</span><span class="sxs-lookup"><span data-stu-id="52b07-164">In this tutorial, SendGrid is used to send email.</span></span> <span data-ttu-id="52b07-165">Você precisa de uma conta do SendGrid e uma chave para enviar email.</span><span class="sxs-lookup"><span data-stu-id="52b07-165">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="52b07-166">Você pode usar outros provedores de email.</span><span class="sxs-lookup"><span data-stu-id="52b07-166">You can use other email providers.</span></span> <span data-ttu-id="52b07-167">ASP.NET Core 2.x inclui `System.Net.Mail`, que permite que você enviar um email de seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="52b07-167">ASP.NET Core 2.x includes `System.Net.Mail`, which allows you to send email from your app.</span></span> <span data-ttu-id="52b07-168">É recomendável que usar o SendGrid ou outro serviço de email para enviar email.</span><span class="sxs-lookup"><span data-stu-id="52b07-168">We recommend you use SendGrid or another email service to send email.</span></span> <span data-ttu-id="52b07-169">SMTP é difícil proteger e configurado corretamente.</span><span class="sxs-lookup"><span data-stu-id="52b07-169">SMTP is difficult to secure and set up correctly.</span></span>

<span data-ttu-id="52b07-170">O [padrão de opções](xref:fundamentals/configuration/options) é usado para acessar as configurações de conta e chave do usuário.</span><span class="sxs-lookup"><span data-stu-id="52b07-170">The [Options pattern](xref:fundamentals/configuration/options) is used to access the user account and key settings.</span></span> <span data-ttu-id="52b07-171">Para obter mais informações, consulte [configuração](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="52b07-171">For more information, see [configuration](xref:fundamentals/configuration/index).</span></span>

<span data-ttu-id="52b07-172">Crie uma classe para buscar a chave de email seguro.</span><span class="sxs-lookup"><span data-stu-id="52b07-172">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="52b07-173">Para este exemplo, o `AuthMessageSenderOptions` classe é criada na *Services/AuthMessageSenderOptions.cs* arquivo:</span><span class="sxs-lookup"><span data-stu-id="52b07-173">For this sample, the `AuthMessageSenderOptions` class is created in the *Services/AuthMessageSenderOptions.cs* file:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/AuthMessageSenderOptions.cs?name=snippet1)]

<span data-ttu-id="52b07-174">Defina as `SendGridUser` e `SendGridKey` com o [ferramenta secret manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="52b07-174">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="52b07-175">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="52b07-175">For example:</span></span>

```console
C:\WebAppl\src\WebApp1>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="52b07-176">No Windows, o Secret Manager armazena os pares de chaves/valor em uma *Secrets* arquivo no `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span><span class="sxs-lookup"><span data-stu-id="52b07-176">On Windows, Secret Manager stores keys/value pairs in a *secrets.json* file in the `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span></span>

<span data-ttu-id="52b07-177">O conteúdo a *Secrets* arquivo não são criptografadas.</span><span class="sxs-lookup"><span data-stu-id="52b07-177">The contents of the *secrets.json* file aren't encrypted.</span></span> <span data-ttu-id="52b07-178">O *Secrets* arquivo é mostrado abaixo (o `SendGridKey` valor tiver sido removido.)</span><span class="sxs-lookup"><span data-stu-id="52b07-178">The *secrets.json* file is shown below (the `SendGridKey` value has been removed.)</span></span>

 ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```

### <a name="configure-startup-to-use-authmessagesenderoptions"></a><span data-ttu-id="52b07-179">Configurar a inicialização para usar AuthMessageSenderOptions</span><span class="sxs-lookup"><span data-stu-id="52b07-179">Configure startup to use AuthMessageSenderOptions</span></span>

<span data-ttu-id="52b07-180">Adicione `AuthMessageSenderOptions` ao contêiner de serviço no final do `ConfigureServices` método na *Startup.cs* arquivo:</span><span class="sxs-lookup"><span data-stu-id="52b07-180">Add `AuthMessageSenderOptions` to the service container at the end of the `ConfigureServices` method in the *Startup.cs* file:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="52b07-181">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="52b07-181">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet2&highlight=28)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="52b07-182">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="52b07-182">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=26)]

---

### <a name="configure-the-authmessagesender-class"></a><span data-ttu-id="52b07-183">Configurar a classe AuthMessageSender</span><span class="sxs-lookup"><span data-stu-id="52b07-183">Configure the AuthMessageSender class</span></span>

<span data-ttu-id="52b07-184">Este tutorial mostra como adicionar notificações de email por meio [SendGrid](https://sendgrid.com/), mas você pode enviar emails usando SMTP e outros mecanismos.</span><span class="sxs-lookup"><span data-stu-id="52b07-184">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

<span data-ttu-id="52b07-185">Instalar o `SendGrid` pacote do NuGet:</span><span class="sxs-lookup"><span data-stu-id="52b07-185">Install the `SendGrid` NuGet package:</span></span>

* <span data-ttu-id="52b07-186">Na linha de comando:</span><span class="sxs-lookup"><span data-stu-id="52b07-186">From the command line:</span></span>

    `dotnet add package SendGrid`

* <span data-ttu-id="52b07-187">No Console do Gerenciador de pacotes, digite o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="52b07-187">From the Package Manager Console, enter the following command:</span></span>

  `Install-Package SendGrid`

<span data-ttu-id="52b07-188">Ver [inicie gratuitamente com o SendGrid](https://sendgrid.com/free/) para se registrar para uma conta gratuita do SendGrid.</span><span class="sxs-lookup"><span data-stu-id="52b07-188">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>

#### <a name="configure-sendgrid"></a><span data-ttu-id="52b07-189">Configure o SendGrid</span><span class="sxs-lookup"><span data-stu-id="52b07-189">Configure SendGrid</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="52b07-190">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="52b07-190">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="52b07-191">Para configurar o SendGrid, adicione o código semelhante ao seguinte no *Services/EmailSender.cs*:</span><span class="sxs-lookup"><span data-stu-id="52b07-191">To configure SendGrid, add code similar to the following in *Services/EmailSender.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/EmailSender.cs)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="52b07-192">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="52b07-192">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

* <span data-ttu-id="52b07-193">Adicione o código em *Services/MessageServices.cs* semelhante ao seguinte para configurar o SendGrid:</span><span class="sxs-lookup"><span data-stu-id="52b07-193">Add code in *Services/MessageServices.cs* similar to the following to configure SendGrid:</span></span>

[!code-csharp[](accconfirm/sample/WebApp1/Services/MessageServices.cs)]

---

## <a name="enable-account-confirmation-and-password-recovery"></a><span data-ttu-id="52b07-194">Habilitar a recuperação de confirmação e a senha da conta</span><span class="sxs-lookup"><span data-stu-id="52b07-194">Enable account confirmation and password recovery</span></span>

<span data-ttu-id="52b07-195">O modelo tem o código para recuperação de confirmação e a senha da conta.</span><span class="sxs-lookup"><span data-stu-id="52b07-195">The template has the code for account confirmation and password recovery.</span></span> <span data-ttu-id="52b07-196">Localizar o `OnPostAsync` método no *Pages/Account/Register.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="52b07-196">Find the `OnPostAsync` method in *Pages/Account/Register.cshtml.cs*.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="52b07-197">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="52b07-197">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="52b07-198">Evitar que usuários recém-registrados seja registrada automaticamente comentando a linha a seguir:</span><span class="sxs-lookup"><span data-stu-id="52b07-198">Prevent newly registered users from being automatically logged on by commenting out the following line:</span></span>

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="52b07-199">O método complete é mostrado com a linha alterada realçada:</span><span class="sxs-lookup"><span data-stu-id="52b07-199">The complete method is shown with the changed line highlighted:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Pages/Account/Register.cshtml.cs?highlight=16&name=snippet_Register)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="52b07-200">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="52b07-200">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="52b07-201">Para habilitar a confirmação de conta, descomente o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="52b07-201">To enable account confirmation, uncomment the following code:</span></span>

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=16-25&name=snippet_Register)]

<span data-ttu-id="52b07-202">**Observação:** o código está impedindo que um usuário registrado recentemente que estão sendo registrados automaticamente pelo comentar a linha a seguir:</span><span class="sxs-lookup"><span data-stu-id="52b07-202">**Note:** The code is preventing a newly registered user from being automatically logged on by commenting out the following line:</span></span>

```csharp
//await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="52b07-203">Habilitar a recuperação de senha removendo o código a `ForgotPassword` ação de *AccountController*:</span><span class="sxs-lookup"><span data-stu-id="52b07-203">Enable password recovery by uncommenting the code in the `ForgotPassword` action of *Controllers/AccountController.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=17-23&name=snippet_ForgotPassword)]

<span data-ttu-id="52b07-204">Remova o elemento de formulário na *Views/Account/ForgotPassword.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="52b07-204">Uncomment the form element in *Views/Account/ForgotPassword.cshtml*.</span></span> <span data-ttu-id="52b07-205">Você talvez queira remover o `<p> For more information on how to enable reset password ... </p>` elemento, que contém um link para este artigo.</span><span class="sxs-lookup"><span data-stu-id="52b07-205">You might want to remove the `<p> For more information on how to enable reset password ... </p>` element, which contains a link to this article.</span></span>

[!code-cshtml[](accconfirm/sample/WebApp1/Views/Account/ForgotPassword.cshtml?highlight=7-10,12,28)]

---

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="52b07-206">Registrar, confirme se o email e redefinição de senha</span><span class="sxs-lookup"><span data-stu-id="52b07-206">Register, confirm email, and reset password</span></span>

<span data-ttu-id="52b07-207">Executar o aplicativo web e testar o fluxo de recuperação de senha e confirmação de conta.</span><span class="sxs-lookup"><span data-stu-id="52b07-207">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="52b07-208">Execute o aplicativo e registrar um novo usuário</span><span class="sxs-lookup"><span data-stu-id="52b07-208">Run the app and register a new user</span></span>

  ![Registrar conta exibição do aplicativo da Web](accconfirm/_static/loginaccconfirm1.png)

* <span data-ttu-id="52b07-210">Verifique seu email para o link de confirmação de conta.</span><span class="sxs-lookup"><span data-stu-id="52b07-210">Check your email for the account confirmation link.</span></span> <span data-ttu-id="52b07-211">Ver [depurar email](#debug) se você não receber o email.</span><span class="sxs-lookup"><span data-stu-id="52b07-211">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="52b07-212">Clique no link para confirmar seu email.</span><span class="sxs-lookup"><span data-stu-id="52b07-212">Click the link to confirm your email.</span></span>
* <span data-ttu-id="52b07-213">Faça logon com seu email e senha.</span><span class="sxs-lookup"><span data-stu-id="52b07-213">Log in with your email and password.</span></span>
* <span data-ttu-id="52b07-214">Faça logoff.</span><span class="sxs-lookup"><span data-stu-id="52b07-214">Log off.</span></span>

### <a name="view-the-manage-page"></a><span data-ttu-id="52b07-215">Exibir a página Gerenciar</span><span class="sxs-lookup"><span data-stu-id="52b07-215">View the manage page</span></span>

<span data-ttu-id="52b07-216">Selecione seu nome de usuário no navegador: ![janela do navegador com o nome de usuário](accconfirm/_static/un.png)</span><span class="sxs-lookup"><span data-stu-id="52b07-216">Select your user name in the browser: ![browser window with user name](accconfirm/_static/un.png)</span></span>

<span data-ttu-id="52b07-217">Talvez seja necessário expandir a barra de navegação para ver o nome de usuário.</span><span class="sxs-lookup"><span data-stu-id="52b07-217">You might need to expand the navbar to see user name.</span></span>

![barra de navegação](accconfirm/_static/x.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="52b07-219">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="52b07-219">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="52b07-220">A página Gerenciar é exibida com o **perfil** guia selecionada.</span><span class="sxs-lookup"><span data-stu-id="52b07-220">The manage page is displayed with the **Profile** tab selected.</span></span> <span data-ttu-id="52b07-221">O **Email** mostra uma caixa de seleção que indica o email foi confirmada.</span><span class="sxs-lookup"><span data-stu-id="52b07-221">The **Email** shows a check box indicating the email has been confirmed.</span></span>

![página Gerenciar](accconfirm/_static/rick2.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="52b07-223">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="52b07-223">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="52b07-224">Isso é mencionado no tutorial posteriormente.</span><span class="sxs-lookup"><span data-stu-id="52b07-224">This is mentioned later in the tutorial.</span></span>
<span data-ttu-id="52b07-225">![página Gerenciar](accconfirm/_static/rick2.png)</span><span class="sxs-lookup"><span data-stu-id="52b07-225">![manage page](accconfirm/_static/rick2.png)</span></span>

---

### <a name="test-password-reset"></a><span data-ttu-id="52b07-226">Teste a redefinição de senha</span><span class="sxs-lookup"><span data-stu-id="52b07-226">Test password reset</span></span>

* <span data-ttu-id="52b07-227">Se você estiver conectado, selecione **Logout**.</span><span class="sxs-lookup"><span data-stu-id="52b07-227">If you're logged in, select **Logout**.</span></span>
* <span data-ttu-id="52b07-228">Selecione o **login** link e selecione o **esqueceu sua senha?** link.</span><span class="sxs-lookup"><span data-stu-id="52b07-228">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="52b07-229">Insira o email usado para registrar a conta.</span><span class="sxs-lookup"><span data-stu-id="52b07-229">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="52b07-230">Um email com um link para redefinir sua senha é enviado.</span><span class="sxs-lookup"><span data-stu-id="52b07-230">An email with a link to reset your password is sent.</span></span> <span data-ttu-id="52b07-231">Verifique seu email e clique no link para redefinir sua senha.</span><span class="sxs-lookup"><span data-stu-id="52b07-231">Check your email and click the link to reset your password.</span></span> <span data-ttu-id="52b07-232">Depois que sua senha foi redefinida com êxito, você pode entrar com seu email e a nova senha.</span><span class="sxs-lookup"><span data-stu-id="52b07-232">After your password has been successfully reset, you can log in with your email and new password.</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="52b07-233">Depurar o email</span><span class="sxs-lookup"><span data-stu-id="52b07-233">Debug email</span></span>

<span data-ttu-id="52b07-234">Se você não é possível obter o trabalho de email:</span><span class="sxs-lookup"><span data-stu-id="52b07-234">If you can't get email working:</span></span>

* <span data-ttu-id="52b07-235">Criar uma [aplicativo de console para enviar email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).</span><span class="sxs-lookup"><span data-stu-id="52b07-235">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).</span></span>
* <span data-ttu-id="52b07-236">Examine os [atividade de Email](https://sendgrid.com/docs/User_Guide/email_activity.html) página.</span><span class="sxs-lookup"><span data-stu-id="52b07-236">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="52b07-237">Verifique sua pasta de spam.</span><span class="sxs-lookup"><span data-stu-id="52b07-237">Check your spam folder.</span></span>
* <span data-ttu-id="52b07-238">Tente outro alias de email em outro provedor de email (Microsoft, Yahoo, Gmail, etc.)</span><span class="sxs-lookup"><span data-stu-id="52b07-238">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="52b07-239">Tente enviar para contas de email diferente.</span><span class="sxs-lookup"><span data-stu-id="52b07-239">Try sending to different email accounts.</span></span>

<span data-ttu-id="52b07-240">**Uma prática recomendada de segurança** é **não** use segredos de produção em desenvolvimento e teste.</span><span class="sxs-lookup"><span data-stu-id="52b07-240">**A security best practice** is to **not** use production secrets in test and development.</span></span> <span data-ttu-id="52b07-241">Se você publicar o aplicativo no Azure, você pode definir os segredos do SendGrid como configurações de aplicativo no portal do aplicativo Web do Azure.</span><span class="sxs-lookup"><span data-stu-id="52b07-241">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="52b07-242">Configurar o sistema de configuração para ler as chaves de variáveis de ambiente.</span><span class="sxs-lookup"><span data-stu-id="52b07-242">The configuration system is set up to read keys from environment variables.</span></span>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="52b07-243">Combine as contas de logon social e local</span><span class="sxs-lookup"><span data-stu-id="52b07-243">Combine social and local login accounts</span></span>

<span data-ttu-id="52b07-244">Para concluir esta seção, você deve primeiro habilitar um provedor de autenticação externa.</span><span class="sxs-lookup"><span data-stu-id="52b07-244">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="52b07-245">Ver [Facebook, Google e a autenticação de provedor externo](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="52b07-245">See [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="52b07-246">Você pode combinar as contas locais e sociais clicando no link seu email.</span><span class="sxs-lookup"><span data-stu-id="52b07-246">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="52b07-247">Na sequência a seguir, "RickAndMSFT@gmail.com" é criado como um logon local; no entanto, você pode criar a conta como um logon social primeiro e adicionar um logon local.</span><span class="sxs-lookup"><span data-stu-id="52b07-247">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Aplicativo Web: RickAndMSFT@gmail.com usuário autenticado](accconfirm/_static/rick.png)

<span data-ttu-id="52b07-249">Clique no **gerenciar** link.</span><span class="sxs-lookup"><span data-stu-id="52b07-249">Click on the **Manage** link.</span></span> <span data-ttu-id="52b07-250">Observe externo 0 (logons sociais) associado a essa conta.</span><span class="sxs-lookup"><span data-stu-id="52b07-250">Note the 0 external (social logins) associated with this account.</span></span>

![Gerenciar o modo de exibição](accconfirm/_static/manage.png)

<span data-ttu-id="52b07-252">Clique no link para outro serviço de logon e aceitar as solicitações do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="52b07-252">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="52b07-253">Na imagem a seguir, o Facebook é o provedor de autenticação externa:</span><span class="sxs-lookup"><span data-stu-id="52b07-253">In the following image, Facebook is the external authentication provider:</span></span>

![Gerenciar seu modo de exibição de logons externos lista Facebook](accconfirm/_static/fb.png)

<span data-ttu-id="52b07-255">As duas contas foram combinadas.</span><span class="sxs-lookup"><span data-stu-id="52b07-255">The two accounts have been combined.</span></span> <span data-ttu-id="52b07-256">É possível fazer logon com qualquer uma das contas.</span><span class="sxs-lookup"><span data-stu-id="52b07-256">You are able to log on with either account.</span></span> <span data-ttu-id="52b07-257">Convém que os usuários adicionem contas locais no caso de seu serviço de autenticação de logon social está inoperante ou, mais provavelmente eles tiver perdido o acesso à sua conta social.</span><span class="sxs-lookup"><span data-stu-id="52b07-257">You might want your users to add local accounts in case their social login authentication service is down, or more likely they've lost access to their social account.</span></span>

## <a name="enable-account-confirmation-after-a-site-has-users"></a><span data-ttu-id="52b07-258">Habilitar confirmação de conta depois que um site tem usuários</span><span class="sxs-lookup"><span data-stu-id="52b07-258">Enable account confirmation after a site has users</span></span>

<span data-ttu-id="52b07-259">Habilitando a confirmação de conta em um site com usuários bloqueia todos os usuários existentes.</span><span class="sxs-lookup"><span data-stu-id="52b07-259">Enabling account confirmation on a site with users locks out all the existing users.</span></span> <span data-ttu-id="52b07-260">Os usuários existentes estão bloqueados porque suas contas não são confirmadas.</span><span class="sxs-lookup"><span data-stu-id="52b07-260">Existing users are locked out because their accounts aren't confirmed.</span></span> <span data-ttu-id="52b07-261">Para contornar o bloqueio de usuário existente, use uma das seguintes abordagens:</span><span class="sxs-lookup"><span data-stu-id="52b07-261">To work around existing user lockout, use one of the following approaches:</span></span>

* <span data-ttu-id="52b07-262">Atualize o banco de dados para marcar todos os usuários existentes como sendo confirmada.</span><span class="sxs-lookup"><span data-stu-id="52b07-262">Update the database to mark all existing users as being confirmed.</span></span>
* <span data-ttu-id="52b07-263">Confirme se os usuários existentes.</span><span class="sxs-lookup"><span data-stu-id="52b07-263">Confirm exiting users.</span></span> <span data-ttu-id="52b07-264">Por exemplo, envio em lote-emails com links de confirmação.</span><span class="sxs-lookup"><span data-stu-id="52b07-264">For example, batch-send emails with confirmation links.</span></span>
