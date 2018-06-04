---
title: Confirmação de conta e de recuperação de senha no ASP.NET Core
author: rick-anderson
description: Saiba como criar um aplicativo do ASP.NET Core com redefinição de senha e de confirmação de email.
manager: wpickett
ms.author: riande
ms.date: 2/11/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/accconfirm
ms.openlocfilehash: b6dbe234973431448c18d3cc82a6ac98d4f53a3b
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34730445"
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a><span data-ttu-id="9dd60-103">Confirmação de conta e de recuperação de senha no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9dd60-103">Account confirmation and password recovery in ASP.NET Core</span></span>

<span data-ttu-id="9dd60-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT) e [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="9dd60-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="9dd60-105">Este tutorial mostra como criar um aplicativo do ASP.NET Core com redefinição de senha e de confirmação de email.</span><span class="sxs-lookup"><span data-stu-id="9dd60-105">This tutorial shows you how to build an ASP.NET Core app with email confirmation and password reset.</span></span> <span data-ttu-id="9dd60-106">Este tutorial é **não** um tópico de início.</span><span class="sxs-lookup"><span data-stu-id="9dd60-106">This tutorial is **not** a beginning topic.</span></span> <span data-ttu-id="9dd60-107">Você deve estar familiarizado com:</span><span class="sxs-lookup"><span data-stu-id="9dd60-107">You should be familiar with:</span></span>

* [<span data-ttu-id="9dd60-108">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9dd60-108">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="9dd60-109">Autenticação</span><span class="sxs-lookup"><span data-stu-id="9dd60-109">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="9dd60-110">Confirmação de conta e recuperação de senha</span><span class="sxs-lookup"><span data-stu-id="9dd60-110">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="9dd60-111">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="9dd60-111">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<span data-ttu-id="9dd60-112">Consulte [este arquivo PDF](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) para as versões do ASP.NET Core MVC 1.1 e 2. x.</span><span class="sxs-lookup"><span data-stu-id="9dd60-112">See [this PDF file](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC 1.1 and 2.x versions.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9dd60-113">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="9dd60-113">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs.md)]

## <a name="create-a-new-aspnet-core-project-with-the-net-core-cli"></a><span data-ttu-id="9dd60-114">Criar um novo projeto do ASP.NET Core com o .NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="9dd60-114">Create a new ASP.NET Core project with the .NET Core CLI</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9dd60-115">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9dd60-115">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new webapp --auth Individual -o WebPWrecover
cd WebPWrecover
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new razor --auth Individual -o WebPWrecover
cd WebPWrecover
```

::: moniker-end

* <span data-ttu-id="9dd60-116">`--auth Individual` Especifica o modelo de projeto de contas de usuário individuais.</span><span class="sxs-lookup"><span data-stu-id="9dd60-116">`--auth Individual` specifies the Individual User Accounts project template.</span></span>
* <span data-ttu-id="9dd60-117">No Windows, adicione o `-uld` opção.</span><span class="sxs-lookup"><span data-stu-id="9dd60-117">On Windows, add the `-uld` option.</span></span> <span data-ttu-id="9dd60-118">Especifica que o LocalDB deve ser usado em vez do SQLite.</span><span class="sxs-lookup"><span data-stu-id="9dd60-118">It specifies LocalDB should be used instead of SQLite.</span></span>
* <span data-ttu-id="9dd60-119">Execute `new mvc --help` para obter ajuda sobre este comando.</span><span class="sxs-lookup"><span data-stu-id="9dd60-119">Run `new mvc --help` to get help on this command.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9dd60-120">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9dd60-120">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="9dd60-121">Se você estiver usando a CLI ou SQLite, execute o seguinte em uma janela de comando:</span><span class="sxs-lookup"><span data-stu-id="9dd60-121">If you're using the CLI or SQLite, run the following in a command window:</span></span>

```console
dotnet new mvc --auth Individual
```

* <span data-ttu-id="9dd60-122">`--auth Individual` Especifica o modelo de projeto de contas de usuário individuais.</span><span class="sxs-lookup"><span data-stu-id="9dd60-122">`--auth Individual` specifies the Individual User Accounts project template.</span></span>
* <span data-ttu-id="9dd60-123">No Windows, adicione o `-uld` opção.</span><span class="sxs-lookup"><span data-stu-id="9dd60-123">On Windows, add the `-uld` option.</span></span> <span data-ttu-id="9dd60-124">Especifica que o LocalDB deve ser usado em vez do SQLite.</span><span class="sxs-lookup"><span data-stu-id="9dd60-124">It specifies LocalDB should be used instead of SQLite.</span></span>
* <span data-ttu-id="9dd60-125">Execute `new mvc --help` para obter ajuda sobre este comando.</span><span class="sxs-lookup"><span data-stu-id="9dd60-125">Run `new mvc --help` to get help on this command.</span></span>

---

<span data-ttu-id="9dd60-126">Como alternativa, você pode criar um novo projeto ASP.NET Core com o Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="9dd60-126">Alternatively, you can create a new ASP.NET Core project with Visual Studio:</span></span>

* <span data-ttu-id="9dd60-127">No Visual Studio, crie um novo **aplicativo Web** projeto.</span><span class="sxs-lookup"><span data-stu-id="9dd60-127">In Visual Studio, create a new **Web Application** project.</span></span>
* <span data-ttu-id="9dd60-128">Selecione **Core ASP.NET 2.0**.</span><span class="sxs-lookup"><span data-stu-id="9dd60-128">Select **ASP.NET Core 2.0**.</span></span> <span data-ttu-id="9dd60-129">**.NET core** está selecionado na imagem a seguir, mas você pode selecionar **do .NET Framework**.</span><span class="sxs-lookup"><span data-stu-id="9dd60-129">**.NET Core** is selected in the following image, but you can select **.NET Framework**.</span></span>
* <span data-ttu-id="9dd60-130">Selecione **alterar autenticação** e definido como **contas de usuário individuais**.</span><span class="sxs-lookup"><span data-stu-id="9dd60-130">Select **Change Authentication** and set to **Individual User Accounts**.</span></span>
* <span data-ttu-id="9dd60-131">Mantenha o padrão **no aplicativo de contas de usuário do repositório**.</span><span class="sxs-lookup"><span data-stu-id="9dd60-131">Keep the default **Store user accounts in-app**.</span></span>

![Nova caixa de diálogo do projeto mostrando "Opção de contas de usuário individuais" selecionada](accconfirm/_static/2.png)

## <a name="test-new-user-registration"></a><span data-ttu-id="9dd60-133">Testar o novo registro de usuário</span><span class="sxs-lookup"><span data-stu-id="9dd60-133">Test new user registration</span></span>

<span data-ttu-id="9dd60-134">Executar o aplicativo, selecione o **registrar** vincular e registrar um usuário.</span><span class="sxs-lookup"><span data-stu-id="9dd60-134">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="9dd60-135">Siga as instruções para executar migrações de Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="9dd60-135">Follow the instructions to run Entity Framework Core migrations.</span></span> <span data-ttu-id="9dd60-136">Neste ponto, a validação somente do email é com o [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) atributo.</span><span class="sxs-lookup"><span data-stu-id="9dd60-136">At this point, the only validation on the email is with the [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attribute.</span></span> <span data-ttu-id="9dd60-137">Depois de enviar o registro, você está conectado no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="9dd60-137">After submitting the registration, you are logged into the app.</span></span> <span data-ttu-id="9dd60-138">Posteriormente no tutorial, o código foi atualizado para que novos usuários não podem fazer logon até que o email foi validado.</span><span class="sxs-lookup"><span data-stu-id="9dd60-138">Later in the tutorial, the code is updated so new users can't log in until their email has been validated.</span></span>

## <a name="view-the-identity-database"></a><span data-ttu-id="9dd60-139">Exibir o banco de dados de identidade</span><span class="sxs-lookup"><span data-stu-id="9dd60-139">View the Identity database</span></span>

<span data-ttu-id="9dd60-140">Consulte [trabalhar com SQLite em um projeto MVC do ASP.NET Core](xref:tutorials/first-mvc-app-xplat/working-with-sql) para obter instruções sobre como exibir o banco de dados SQLite.</span><span class="sxs-lookup"><span data-stu-id="9dd60-140">See [Work with SQLite in an ASP.NET Core MVC project](xref:tutorials/first-mvc-app-xplat/working-with-sql) for instructions on how to view the SQLite database.</span></span>

<span data-ttu-id="9dd60-141">Para o Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="9dd60-141">For Visual Studio:</span></span>

* <span data-ttu-id="9dd60-142">Do **exibição** menu, selecione **Pesquisador de objetos do SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="9dd60-142">From the **View** menu, select **SQL Server Object Explorer** (SSOX).</span></span>
* <span data-ttu-id="9dd60-143">Navegue até **(localdb) MSSQLLocalDB (SQL Server 13)**.</span><span class="sxs-lookup"><span data-stu-id="9dd60-143">Navigate to **(localdb)MSSQLLocalDB(SQL Server 13)**.</span></span> <span data-ttu-id="9dd60-144">Clique duas vezes em **dbo. AspNetUsers** > **exibir dados**:</span><span class="sxs-lookup"><span data-stu-id="9dd60-144">Right-click on **dbo.AspNetUsers** > **View Data**:</span></span>

![Menu de contexto no AspNetUsers tabela no Pesquisador de objetos do SQL Server](accconfirm/_static/ssox.png)

<span data-ttu-id="9dd60-146">Observe a tabela `EmailConfirmed` campo é `False`.</span><span class="sxs-lookup"><span data-stu-id="9dd60-146">Note the table's `EmailConfirmed` field is `False`.</span></span>

<span data-ttu-id="9dd60-147">Você talvez queira usar o email novamente na próxima etapa, quando o aplicativo envia um email de confirmação.</span><span class="sxs-lookup"><span data-stu-id="9dd60-147">You might want to use this email again in the next step when the app sends a confirmation email.</span></span> <span data-ttu-id="9dd60-148">Clique na linha e selecione **excluir**.</span><span class="sxs-lookup"><span data-stu-id="9dd60-148">Right-click on the row and select **Delete**.</span></span> <span data-ttu-id="9dd60-149">Excluir o alias de email torna mais fácil nas etapas a seguir.</span><span class="sxs-lookup"><span data-stu-id="9dd60-149">Deleting the email alias makes it easier in the following steps.</span></span>

---

## <a name="require-https"></a><span data-ttu-id="9dd60-150">Exigir HTTPS</span><span class="sxs-lookup"><span data-stu-id="9dd60-150">Require HTTPS</span></span>

<span data-ttu-id="9dd60-151">Consulte [exigir HTTPS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="9dd60-151">See [Require HTTPS](xref:security/enforcing-ssl).</span></span>

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a><span data-ttu-id="9dd60-152">Solicitar confirmação de email</span><span class="sxs-lookup"><span data-stu-id="9dd60-152">Require email confirmation</span></span>

<span data-ttu-id="9dd60-153">É uma prática recomendada para confirmar o email de um novo registro de usuário.</span><span class="sxs-lookup"><span data-stu-id="9dd60-153">It's a best practice to confirm the email of a new user registration.</span></span> <span data-ttu-id="9dd60-154">Ajuda de confirmação para verificar se eles não estiver representando alguém de email (ou seja, eles ainda não registrados com outra pessoa email).</span><span class="sxs-lookup"><span data-stu-id="9dd60-154">Email confirmation helps to verify they're not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="9dd60-155">Suponha que você tivesse um fórum de discussão, e quiser impedir "yli@example.com"do registro como"nolivetto@contoso.com".</span><span class="sxs-lookup"><span data-stu-id="9dd60-155">Suppose you had a discussion forum, and you wanted to prevent "yli@example.com" from registering as "nolivetto@contoso.com".</span></span> <span data-ttu-id="9dd60-156">Sem confirmação por email, "nolivetto@contoso.com" pode receber email indesejado de seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="9dd60-156">Without email confirmation, "nolivetto@contoso.com" could receive unwanted email from your app.</span></span> <span data-ttu-id="9dd60-157">Suponha que o usuário registrado acidentalmente como "ylo@example.com" e ainda não tenha percebido a digitação incorreta da "yli".</span><span class="sxs-lookup"><span data-stu-id="9dd60-157">Suppose the user accidentally registered as "ylo@example.com" and hadn't noticed the misspelling of "yli".</span></span> <span data-ttu-id="9dd60-158">Elas não serão capazes de usar a recuperação de senha porque o aplicativo não tiver seu email correto.</span><span class="sxs-lookup"><span data-stu-id="9dd60-158">They wouldn't be able to use password recovery because the app doesn't have their correct email.</span></span> <span data-ttu-id="9dd60-159">Email de confirmação oferece apenas proteção limitada de robôs.</span><span class="sxs-lookup"><span data-stu-id="9dd60-159">Email confirmation provides only limited protection from bots.</span></span> <span data-ttu-id="9dd60-160">Email de confirmação não fornece proteção contra usuários mal-intencionados com várias contas de email.</span><span class="sxs-lookup"><span data-stu-id="9dd60-160">Email confirmation doesn't provide protection from malicious users with many email accounts.</span></span>

<span data-ttu-id="9dd60-161">Geralmente você deseja impedir que novos usuários incluam dados em seu site até que eles tenham um email confirmado.</span><span class="sxs-lookup"><span data-stu-id="9dd60-161">You generally want to prevent new users from posting any data to your web site before they have a confirmed email.</span></span>

<span data-ttu-id="9dd60-162">Atualização `ConfigureServices` para exigir um email confirmado:</span><span class="sxs-lookup"><span data-stu-id="9dd60-162">Update `ConfigureServices` to require a confirmed email:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet1&highlight=12-17)]

<span data-ttu-id="9dd60-163">`config.SignIn.RequireConfirmedEmail = true;` impede que usuários registrados fazer logon até que o email foi confirmado.</span><span class="sxs-lookup"><span data-stu-id="9dd60-163">`config.SignIn.RequireConfirmedEmail = true;` prevents registered users from logging in until their email is confirmed.</span></span>

### <a name="configure-email-provider"></a><span data-ttu-id="9dd60-164">Configurar o provedor de email</span><span class="sxs-lookup"><span data-stu-id="9dd60-164">Configure email provider</span></span>

<span data-ttu-id="9dd60-165">Neste tutorial, SendGrid é usada para enviar email.</span><span class="sxs-lookup"><span data-stu-id="9dd60-165">In this tutorial, SendGrid is used to send email.</span></span> <span data-ttu-id="9dd60-166">Você precisa de uma conta do SendGrid e a chave para enviar email.</span><span class="sxs-lookup"><span data-stu-id="9dd60-166">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="9dd60-167">Você pode usar outros provedores de email.</span><span class="sxs-lookup"><span data-stu-id="9dd60-167">You can use other email providers.</span></span> <span data-ttu-id="9dd60-168">ASP.NET Core 2. x inclui `System.Net.Mail`, que permite enviar email de seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="9dd60-168">ASP.NET Core 2.x includes `System.Net.Mail`, which allows you to send email from your app.</span></span> <span data-ttu-id="9dd60-169">É recomendável que usar o SendGrid ou outro serviço de email para enviar email.</span><span class="sxs-lookup"><span data-stu-id="9dd60-169">We recommend you use SendGrid or another email service to send email.</span></span> <span data-ttu-id="9dd60-170">O SMTP é difícil proteger e configurado corretamente.</span><span class="sxs-lookup"><span data-stu-id="9dd60-170">SMTP is difficult to secure and set up correctly.</span></span>

<span data-ttu-id="9dd60-171">O [padrão de opções](xref:fundamentals/configuration/options) é usado para acessar as configurações de conta e a chave de usuário.</span><span class="sxs-lookup"><span data-stu-id="9dd60-171">The [Options pattern](xref:fundamentals/configuration/options) is used to access the user account and key settings.</span></span> <span data-ttu-id="9dd60-172">Para obter mais informações, consulte [configuração](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="9dd60-172">For more information, see [configuration](xref:fundamentals/configuration/index).</span></span>

<span data-ttu-id="9dd60-173">Crie uma classe para obter a chave de email seguro.</span><span class="sxs-lookup"><span data-stu-id="9dd60-173">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="9dd60-174">Para este exemplo, o `AuthMessageSenderOptions` classe é criada no *Services/AuthMessageSenderOptions.cs* arquivo:</span><span class="sxs-lookup"><span data-stu-id="9dd60-174">For this sample, the `AuthMessageSenderOptions` class is created in the *Services/AuthMessageSenderOptions.cs* file:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/AuthMessageSenderOptions.cs?name=snippet1)]

<span data-ttu-id="9dd60-175">Definir o `SendGridUser` e `SendGridKey` com o [ferramenta Gerenciador de segredo](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="9dd60-175">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="9dd60-176">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="9dd60-176">For example:</span></span>

```console
C:\WebAppl\src\WebApp1>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="9dd60-177">No Windows, o segredo Manager armazena pares de chaves/valor em uma *secrets.json* arquivo o `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span><span class="sxs-lookup"><span data-stu-id="9dd60-177">On Windows, Secret Manager stores keys/value pairs in a *secrets.json* file in the `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span></span>

<span data-ttu-id="9dd60-178">O conteúdo do *secrets.json* arquivo não são criptografados.</span><span class="sxs-lookup"><span data-stu-id="9dd60-178">The contents of the *secrets.json* file aren't encrypted.</span></span> <span data-ttu-id="9dd60-179">O *secrets.json* arquivo é mostrado a seguir (o `SendGridKey` valor foi removido.)</span><span class="sxs-lookup"><span data-stu-id="9dd60-179">The *secrets.json* file is shown below (the `SendGridKey` value has been removed.)</span></span>

 ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```

### <a name="configure-startup-to-use-authmessagesenderoptions"></a><span data-ttu-id="9dd60-180">Configurar a inicialização para usar AuthMessageSenderOptions</span><span class="sxs-lookup"><span data-stu-id="9dd60-180">Configure startup to use AuthMessageSenderOptions</span></span>

<span data-ttu-id="9dd60-181">Adicionar `AuthMessageSenderOptions` ao contêiner de serviço no final o `ConfigureServices` método o *Startup.cs* arquivo:</span><span class="sxs-lookup"><span data-stu-id="9dd60-181">Add `AuthMessageSenderOptions` to the service container at the end of the `ConfigureServices` method in the *Startup.cs* file:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9dd60-182">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9dd60-182">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet2&highlight=28)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9dd60-183">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9dd60-183">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=26)]

---

### <a name="configure-the-authmessagesender-class"></a><span data-ttu-id="9dd60-184">Configurar a classe AuthMessageSender</span><span class="sxs-lookup"><span data-stu-id="9dd60-184">Configure the AuthMessageSender class</span></span>

<span data-ttu-id="9dd60-185">Este tutorial mostra como adicionar notificações de email por meio de [SendGrid](https://sendgrid.com/), mas você pode enviar emails usando SMTP e outros mecanismos.</span><span class="sxs-lookup"><span data-stu-id="9dd60-185">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

<span data-ttu-id="9dd60-186">Instalar o `SendGrid` pacote do NuGet:</span><span class="sxs-lookup"><span data-stu-id="9dd60-186">Install the `SendGrid` NuGet package:</span></span>

* <span data-ttu-id="9dd60-187">Na linha de comando:</span><span class="sxs-lookup"><span data-stu-id="9dd60-187">From the command line:</span></span>

    `dotnet add package SendGrid`

* <span data-ttu-id="9dd60-188">No Console do Gerenciador de pacotes, digite o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="9dd60-188">From the Package Manager Console, enter the following command:</span></span>

  `Install-Package SendGrid`

<span data-ttu-id="9dd60-189">Consulte [comece com SendGrid gratuitamente](https://sendgrid.com/free/) para registrar-se para uma conta gratuita do SendGrid.</span><span class="sxs-lookup"><span data-stu-id="9dd60-189">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>

#### <a name="configure-sendgrid"></a><span data-ttu-id="9dd60-190">Configurar o SendGrid</span><span class="sxs-lookup"><span data-stu-id="9dd60-190">Configure SendGrid</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9dd60-191">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9dd60-191">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="9dd60-192">Para configurar o SendGrid, adicione o código semelhante ao seguinte no *Services/EmailSender.cs*:</span><span class="sxs-lookup"><span data-stu-id="9dd60-192">To configure SendGrid, add code similar to the following in *Services/EmailSender.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/EmailSender.cs)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9dd60-193">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9dd60-193">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

* <span data-ttu-id="9dd60-194">Adicione código *Services/MessageServices.cs* semelhante à seguinte para configurar o SendGrid:</span><span class="sxs-lookup"><span data-stu-id="9dd60-194">Add code in *Services/MessageServices.cs* similar to the following to configure SendGrid:</span></span>

[!code-csharp[](accconfirm/sample/WebApp1/Services/MessageServices.cs)]

---

## <a name="enable-account-confirmation-and-password-recovery"></a><span data-ttu-id="9dd60-195">Habilitar a recuperação de confirmação e a senha da conta</span><span class="sxs-lookup"><span data-stu-id="9dd60-195">Enable account confirmation and password recovery</span></span>

<span data-ttu-id="9dd60-196">O modelo tem o código de recuperação de confirmação e a senha da conta.</span><span class="sxs-lookup"><span data-stu-id="9dd60-196">The template has the code for account confirmation and password recovery.</span></span> <span data-ttu-id="9dd60-197">Localizar o `OnPostAsync` método *Pages/Account/Register.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="9dd60-197">Find the `OnPostAsync` method in *Pages/Account/Register.cshtml.cs*.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9dd60-198">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9dd60-198">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="9dd60-199">Impedi que usuários recém-registrados sendo registrados automaticamente pelo comentar a seguinte linha:</span><span class="sxs-lookup"><span data-stu-id="9dd60-199">Prevent newly registered users from being automatically logged on by commenting out the following line:</span></span>

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="9dd60-200">O método complete é mostrado com a linha alterada realçada:</span><span class="sxs-lookup"><span data-stu-id="9dd60-200">The complete method is shown with the changed line highlighted:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Pages/Account/Register.cshtml.cs?highlight=16&name=snippet_Register)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9dd60-201">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9dd60-201">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="9dd60-202">Para habilitar a confirmação de conta, descomente o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="9dd60-202">To enable account confirmation, uncomment the following code:</span></span>

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=16-25&name=snippet_Register)]

<span data-ttu-id="9dd60-203">**Observação:** o código está impedindo que um usuário registrado recentemente que estão sendo registrados automaticamente pelo comentar a seguinte linha:</span><span class="sxs-lookup"><span data-stu-id="9dd60-203">**Note:** The code is preventing a newly registered user from being automatically logged on by commenting out the following line:</span></span>

```csharp
//await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="9dd60-204">Habilitar a recuperação de senha por uncommenting o código de `ForgotPassword` ação de *Controllers/AccountController.cs*:</span><span class="sxs-lookup"><span data-stu-id="9dd60-204">Enable password recovery by uncommenting the code in the `ForgotPassword` action of *Controllers/AccountController.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=17-23&name=snippet_ForgotPassword)]

<span data-ttu-id="9dd60-205">Remova o elemento de formulário em *Views/Account/ForgotPassword.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="9dd60-205">Uncomment the form element in *Views/Account/ForgotPassword.cshtml*.</span></span> <span data-ttu-id="9dd60-206">Talvez você queira remover o `<p> For more information on how to enable reset password ... </p>` elemento, que contém um link para este artigo.</span><span class="sxs-lookup"><span data-stu-id="9dd60-206">You might want to remove the `<p> For more information on how to enable reset password ... </p>` element, which contains a link to this article.</span></span>

[!code-cshtml[](accconfirm/sample/WebApp1/Views/Account/ForgotPassword.cshtml?highlight=7-10,12,28)]

---

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="9dd60-207">Registrar, confirme o email e redefinição de senha</span><span class="sxs-lookup"><span data-stu-id="9dd60-207">Register, confirm email, and reset password</span></span>

<span data-ttu-id="9dd60-208">Executar o aplicativo web e testar o fluxo de recuperação de senha e confirmação de conta.</span><span class="sxs-lookup"><span data-stu-id="9dd60-208">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="9dd60-209">Execute o aplicativo e registrar um novo usuário</span><span class="sxs-lookup"><span data-stu-id="9dd60-209">Run the app and register a new user</span></span>

  ![Registrar conta exibição do aplicativo Web](accconfirm/_static/loginaccconfirm1.png)

* <span data-ttu-id="9dd60-211">Verifique seu email para o link de confirmação de conta.</span><span class="sxs-lookup"><span data-stu-id="9dd60-211">Check your email for the account confirmation link.</span></span> <span data-ttu-id="9dd60-212">Consulte [depurar email](#debug) se você não receber o email.</span><span class="sxs-lookup"><span data-stu-id="9dd60-212">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="9dd60-213">Clique no link para confirmar seu email.</span><span class="sxs-lookup"><span data-stu-id="9dd60-213">Click the link to confirm your email.</span></span>
* <span data-ttu-id="9dd60-214">Faça logon com seu email e senha.</span><span class="sxs-lookup"><span data-stu-id="9dd60-214">Log in with your email and password.</span></span>
* <span data-ttu-id="9dd60-215">Fazer logoff.</span><span class="sxs-lookup"><span data-stu-id="9dd60-215">Log off.</span></span>

### <a name="view-the-manage-page"></a><span data-ttu-id="9dd60-216">Exibir a página de gerenciamento</span><span class="sxs-lookup"><span data-stu-id="9dd60-216">View the manage page</span></span>

<span data-ttu-id="9dd60-217">Selecione o nome de usuário no navegador: ![janela do navegador com o nome de usuário](accconfirm/_static/un.png)</span><span class="sxs-lookup"><span data-stu-id="9dd60-217">Select your user name in the browser: ![browser window with user name](accconfirm/_static/un.png)</span></span>

<span data-ttu-id="9dd60-218">Talvez seja necessário expandir a barra de navegação para ver o nome de usuário.</span><span class="sxs-lookup"><span data-stu-id="9dd60-218">You might need to expand the navbar to see user name.</span></span>

![barra de navegação](accconfirm/_static/x.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9dd60-220">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9dd60-220">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="9dd60-221">A página de gerenciamento é exibida com o **perfil** guia selecionada.</span><span class="sxs-lookup"><span data-stu-id="9dd60-221">The manage page is displayed with the **Profile** tab selected.</span></span> <span data-ttu-id="9dd60-222">O **Email** mostra uma caixa de seleção que indica o email foi confirmada.</span><span class="sxs-lookup"><span data-stu-id="9dd60-222">The **Email** shows a check box indicating the email has been confirmed.</span></span>

![página Gerenciar](accconfirm/_static/rick2.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9dd60-224">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9dd60-224">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="9dd60-225">Isto é mencionado no tutorial posteriormente.</span><span class="sxs-lookup"><span data-stu-id="9dd60-225">This is mentioned later in the tutorial.</span></span>
<span data-ttu-id="9dd60-226">![página Gerenciar](accconfirm/_static/rick2.png)</span><span class="sxs-lookup"><span data-stu-id="9dd60-226">![manage page](accconfirm/_static/rick2.png)</span></span>

---

### <a name="test-password-reset"></a><span data-ttu-id="9dd60-227">Redefinição de senha do teste</span><span class="sxs-lookup"><span data-stu-id="9dd60-227">Test password reset</span></span>

* <span data-ttu-id="9dd60-228">Se você estiver conectado, selecione **Logout**.</span><span class="sxs-lookup"><span data-stu-id="9dd60-228">If you're logged in, select **Logout**.</span></span>
* <span data-ttu-id="9dd60-229">Selecione o **login** link e selecione o **esqueceu sua senha?** link.</span><span class="sxs-lookup"><span data-stu-id="9dd60-229">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="9dd60-230">Insira o email que é usado para registrar a conta.</span><span class="sxs-lookup"><span data-stu-id="9dd60-230">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="9dd60-231">É enviado um email com um link para redefinir sua senha.</span><span class="sxs-lookup"><span data-stu-id="9dd60-231">An email with a link to reset your password is sent.</span></span> <span data-ttu-id="9dd60-232">Verifique seu email e clique no link para redefinir sua senha.</span><span class="sxs-lookup"><span data-stu-id="9dd60-232">Check your email and click the link to reset your password.</span></span> <span data-ttu-id="9dd60-233">Depois que sua senha foi redefinida com êxito, você pode fazer logon com seu email e a nova senha.</span><span class="sxs-lookup"><span data-stu-id="9dd60-233">After your password has been successfully reset, you can log in with your email and new password.</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="9dd60-234">Depurar email</span><span class="sxs-lookup"><span data-stu-id="9dd60-234">Debug email</span></span>

<span data-ttu-id="9dd60-235">Se você não pode receber email de trabalho:</span><span class="sxs-lookup"><span data-stu-id="9dd60-235">If you can't get email working:</span></span>

* <span data-ttu-id="9dd60-236">Criar um [aplicativo de console para enviar email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).</span><span class="sxs-lookup"><span data-stu-id="9dd60-236">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).</span></span>
* <span data-ttu-id="9dd60-237">Examine o [atividade Email](https://sendgrid.com/docs/User_Guide/email_activity.html) página.</span><span class="sxs-lookup"><span data-stu-id="9dd60-237">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="9dd60-238">Verifique a pasta de spam.</span><span class="sxs-lookup"><span data-stu-id="9dd60-238">Check your spam folder.</span></span>
* <span data-ttu-id="9dd60-239">Tente outro alias de email em um provedor de email diferente (Microsoft, Yahoo, Gmail, etc.)</span><span class="sxs-lookup"><span data-stu-id="9dd60-239">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="9dd60-240">Tente enviar para contas de email diferente.</span><span class="sxs-lookup"><span data-stu-id="9dd60-240">Try sending to different email accounts.</span></span>

<span data-ttu-id="9dd60-241">**Uma prática recomendada de segurança** é **não** use segredos de produção no desenvolvimento e teste.</span><span class="sxs-lookup"><span data-stu-id="9dd60-241">**A security best practice** is to **not** use production secrets in test and development.</span></span> <span data-ttu-id="9dd60-242">Se você publicar o aplicativo no Azure, você pode definir os segredos do SendGrid como configurações de aplicativo no portal do aplicativo Web do Azure.</span><span class="sxs-lookup"><span data-stu-id="9dd60-242">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="9dd60-243">O sistema de configuração é configurado para ler as chaves de variáveis de ambiente.</span><span class="sxs-lookup"><span data-stu-id="9dd60-243">The configuration system is set up to read keys from environment variables.</span></span>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="9dd60-244">Combinar as contas de logon local e social</span><span class="sxs-lookup"><span data-stu-id="9dd60-244">Combine social and local login accounts</span></span>

<span data-ttu-id="9dd60-245">Para concluir esta seção, você deve primeiro habilitar um provedor de autenticação externa.</span><span class="sxs-lookup"><span data-stu-id="9dd60-245">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="9dd60-246">Consulte [Facebook, Google e a autenticação do provedor externo](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="9dd60-246">See [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="9dd60-247">Você pode combinar as contas locais e sociais clicando no link seu email.</span><span class="sxs-lookup"><span data-stu-id="9dd60-247">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="9dd60-248">Na sequência a seguir, "RickAndMSFT@gmail.com" é criado como um logon local; no entanto, você pode criar a conta como um logon social primeiro, depois de adicionar um logon local.</span><span class="sxs-lookup"><span data-stu-id="9dd60-248">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Aplicativo Web: RickAndMSFT@gmail.com usuário autenticado](accconfirm/_static/rick.png)

<span data-ttu-id="9dd60-250">Clique no **gerenciar** link.</span><span class="sxs-lookup"><span data-stu-id="9dd60-250">Click on the **Manage** link.</span></span> <span data-ttu-id="9dd60-251">Observe externo 0 (logons sociais) associada à conta.</span><span class="sxs-lookup"><span data-stu-id="9dd60-251">Note the 0 external (social logins) associated with this account.</span></span>

![Gerenciar o modo de exibição](accconfirm/_static/manage.png)

<span data-ttu-id="9dd60-253">Clique no link para outro serviço de logon e aceitar as solicitações do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="9dd60-253">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="9dd60-254">Na imagem a seguir, o Facebook é o provedor de autenticação externa:</span><span class="sxs-lookup"><span data-stu-id="9dd60-254">In the following image, Facebook is the external authentication provider:</span></span>

![Gerenciar o modo de exibição de logons externos listando Facebook](accconfirm/_static/fb.png)

<span data-ttu-id="9dd60-256">As duas contas foram combinadas.</span><span class="sxs-lookup"><span data-stu-id="9dd60-256">The two accounts have been combined.</span></span> <span data-ttu-id="9dd60-257">É possível fazer logon com a conta.</span><span class="sxs-lookup"><span data-stu-id="9dd60-257">You are able to log on with either account.</span></span> <span data-ttu-id="9dd60-258">Convém que os usuários adicionem contas locais, caso seu serviço de autenticação de logon social está inoperante ou mais provável perdeu o acesso à sua conta social.</span><span class="sxs-lookup"><span data-stu-id="9dd60-258">You might want your users to add local accounts in case their social login authentication service is down, or more likely they've lost access to their social account.</span></span>

## <a name="enable-account-confirmation-after-a-site-has-users"></a><span data-ttu-id="9dd60-259">Habilitar confirmação de conta após um site tem usuários</span><span class="sxs-lookup"><span data-stu-id="9dd60-259">Enable account confirmation after a site has users</span></span>

<span data-ttu-id="9dd60-260">Habilitar confirmação de conta em um site com usuários bloqueia todos os usuários existentes.</span><span class="sxs-lookup"><span data-stu-id="9dd60-260">Enabling account confirmation on a site with users locks out all the existing users.</span></span> <span data-ttu-id="9dd60-261">Os usuários estão bloqueados porque suas contas não são confirmadas.</span><span class="sxs-lookup"><span data-stu-id="9dd60-261">Existing users are locked out because their accounts aren't confirmed.</span></span> <span data-ttu-id="9dd60-262">Para contornar o bloqueio de usuário existente, use uma das seguintes abordagens:</span><span class="sxs-lookup"><span data-stu-id="9dd60-262">To work around existing user lockout, use one of the following approaches:</span></span>

* <span data-ttu-id="9dd60-263">Atualize o banco de dados para marcar todos os usuários existentes como sendo confirmada.</span><span class="sxs-lookup"><span data-stu-id="9dd60-263">Update the database to mark all existing users as being confirmed.</span></span>
* <span data-ttu-id="9dd60-264">Confirme se os usuários existentes.</span><span class="sxs-lookup"><span data-stu-id="9dd60-264">Confirm exiting users.</span></span> <span data-ttu-id="9dd60-265">Por exemplo, envio em lote-emails com links de confirmação.</span><span class="sxs-lookup"><span data-stu-id="9dd60-265">For example, batch-send emails with confirmation links.</span></span>
