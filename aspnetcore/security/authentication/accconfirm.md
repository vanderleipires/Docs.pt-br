---
title: Confirmação de conta e de recuperação de senha no ASP.NET Core
author: rick-anderson
description: Saiba como criar um aplicativo ASP.NET Core com a redefinição de senha e de confirmação de email.
ms.author: riande
ms.date: 7/11/2018
uid: security/authentication/accconfirm
ms.openlocfilehash: 84eb3580107572f66f0c3b565b8e76ba401c0ddb
ms.sourcegitcommit: 8f8924ce4eb9effeaf489f177fb01b66867da16f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2018
ms.locfileid: "39219401"
---
::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="9bf92-103">Ver [esse arquivo PDF](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) para o ASP.NET Core 1.1 e a versão 2.1.</span><span class="sxs-lookup"><span data-stu-id="9bf92-103">See [this PDF file](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core 1.1 and 2.1 version.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a><span data-ttu-id="9bf92-104">Confirmação de conta e de recuperação de senha no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9bf92-104">Account confirmation and password recovery in ASP.NET Core</span></span>

<span data-ttu-id="9bf92-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT) e [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="9bf92-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="9bf92-106">Este tutorial mostra como criar um aplicativo ASP.NET Core com a redefinição de senha e de confirmação de email.</span><span class="sxs-lookup"><span data-stu-id="9bf92-106">This tutorial shows how to build an ASP.NET Core app with email confirmation and password reset.</span></span> <span data-ttu-id="9bf92-107">Este tutorial é **não** um tópico de início.</span><span class="sxs-lookup"><span data-stu-id="9bf92-107">This tutorial is **not** a beginning topic.</span></span> <span data-ttu-id="9bf92-108">Você deve estar familiarizado com:</span><span class="sxs-lookup"><span data-stu-id="9bf92-108">You should be familiar with:</span></span>

* [<span data-ttu-id="9bf92-109">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9bf92-109">ASP.NET Core</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="9bf92-110">Autenticação</span><span class="sxs-lookup"><span data-stu-id="9bf92-110">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="9bf92-111">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="9bf92-111">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<!-- see C:/Dropbox/wrk/Code/SendGridConsole/Program.cs -->

## <a name="prerequisites"></a><span data-ttu-id="9bf92-112">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="9bf92-112">Prerequisites</span></span>

[!INCLUDE [](~/includes/2.1-SDK.md)]

## <a name="create-a-web--app-and-scaffold-identity"></a><span data-ttu-id="9bf92-113">Criar um aplicativo web e o scaffolding de identidade</span><span class="sxs-lookup"><span data-stu-id="9bf92-113">Create a web  app and scaffold Identity</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9bf92-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9bf92-114">Visual Studio</span></span>](#tab/visual-studio) 

* <span data-ttu-id="9bf92-115">No Visual Studio, crie um novo **aplicativo Web** projeto chamado **WebPWrecover**.</span><span class="sxs-lookup"><span data-stu-id="9bf92-115">In Visual Studio, create a new **Web Application** project named **WebPWrecover**.</span></span>
* <span data-ttu-id="9bf92-116">Selecione **ASP.NET Core 2.1**.</span><span class="sxs-lookup"><span data-stu-id="9bf92-116">Select **ASP.NET Core 2.1**.</span></span>
* <span data-ttu-id="9bf92-117">Mantenha o padrão **autenticação** definido como **sem autenticação**.</span><span class="sxs-lookup"><span data-stu-id="9bf92-117">Keep the default **Authentication** set to **No Authentication**.</span></span> <span data-ttu-id="9bf92-118">Autenticação é adicionada na próxima etapa.</span><span class="sxs-lookup"><span data-stu-id="9bf92-118">Authentication is added in the next step.</span></span>

<span data-ttu-id="9bf92-119">Na próxima etapa:</span><span class="sxs-lookup"><span data-stu-id="9bf92-119">In the next step:</span></span>

* <span data-ttu-id="9bf92-120">Definir a página de layout *~/Pages/Shared/_Layout.cshtml*</span><span class="sxs-lookup"><span data-stu-id="9bf92-120">Set the layout page to *~/Pages/Shared/_Layout.cshtml*</span></span>
* <span data-ttu-id="9bf92-121">Selecione *conta/registro*</span><span class="sxs-lookup"><span data-stu-id="9bf92-121">Select *Account/Register*</span></span>
* <span data-ttu-id="9bf92-122">Criar um novo **classe de contexto de dados**</span><span class="sxs-lookup"><span data-stu-id="9bf92-122">Create a new **Data context class**</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="9bf92-123">CLI do .NET Core</span><span class="sxs-lookup"><span data-stu-id="9bf92-123">.NET Core CLI</span></span>](#tab/netcore-cli)

```console
dotnet new webapp -o WebPWrecover
cd WebPWrecover
dotnet tool install -g dotnet-aspnet-codegenerator
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -fi Account.Register -dc WebPWrecover.Models.WebPWrecoverContext
dotnet ef migrations add CreateIdentitySchema
dotnet ef database drop -f
dotnet ef database update
dotnet build
```

<span data-ttu-id="9bf92-124">Executar `dotnet aspnet-codegenerator identity --help` para obter ajuda sobre a ferramenta de scaffolding.</span><span class="sxs-lookup"><span data-stu-id="9bf92-124">Run `dotnet aspnet-codegenerator identity --help` to get help on the scaffolding tool.</span></span>

------

<span data-ttu-id="9bf92-125">Siga as instruções em [habilitar a autenticação](xref:security/authentication/scaffold-identity#useauthentication):</span><span class="sxs-lookup"><span data-stu-id="9bf92-125">Follow the instructions in [Enable authentication](xref:security/authentication/scaffold-identity#useauthentication):</span></span>

* <span data-ttu-id="9bf92-126">Adicionar `app.UseAuthentication();` para `Startup.Configure`</span><span class="sxs-lookup"><span data-stu-id="9bf92-126">Add `app.UseAuthentication();` to `Startup.Configure`</span></span>
* <span data-ttu-id="9bf92-127">Adicionar `<partial name="_LoginPartial" />` ao arquivo de layout.</span><span class="sxs-lookup"><span data-stu-id="9bf92-127">Add `<partial name="_LoginPartial" />` to the layout file.</span></span>

## <a name="test-new-user-registration"></a><span data-ttu-id="9bf92-128">Novo registro de usuário de teste</span><span class="sxs-lookup"><span data-stu-id="9bf92-128">Test new user registration</span></span>

<span data-ttu-id="9bf92-129">Execute o aplicativo, selecione a **registrar** vincular e registrar um usuário.</span><span class="sxs-lookup"><span data-stu-id="9bf92-129">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="9bf92-130">Neste ponto, a validação apenas no email é com o [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) atributo.</span><span class="sxs-lookup"><span data-stu-id="9bf92-130">At this point, the only validation on the email is with the [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attribute.</span></span> <span data-ttu-id="9bf92-131">Depois de enviar o registro, você está conectado ao aplicativo.</span><span class="sxs-lookup"><span data-stu-id="9bf92-131">After submitting the registration, you are logged into the app.</span></span> <span data-ttu-id="9bf92-132">Posteriormente no tutorial, o código é atualizado para que novos usuários não podem fazer logon até que o email é validado.</span><span class="sxs-lookup"><span data-stu-id="9bf92-132">Later in the tutorial, the code is updated so new users can't log in until their email is validated.</span></span>

## <a name="view-the-identity-database"></a><span data-ttu-id="9bf92-133">Exibir o banco de dados de identidade</span><span class="sxs-lookup"><span data-stu-id="9bf92-133">View the Identity database</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9bf92-134">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9bf92-134">Visual Studio</span></span>](#tab/visual-studio) 

* <span data-ttu-id="9bf92-135">Dos **modo de exibição** menu, selecione **Pesquisador de objetos do SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="9bf92-135">From the **View** menu, select **SQL Server Object Explorer** (SSOX).</span></span>
* <span data-ttu-id="9bf92-136">Navegue até **(localdb) MSSQLLocalDB (SQL Server 13)**.</span><span class="sxs-lookup"><span data-stu-id="9bf92-136">Navigate to **(localdb)MSSQLLocalDB(SQL Server 13)**.</span></span> <span data-ttu-id="9bf92-137">Clique duas vezes em **dbo. AspNetUsers** > **exibir dados**:</span><span class="sxs-lookup"><span data-stu-id="9bf92-137">Right-click on **dbo.AspNetUsers** > **View Data**:</span></span>

![Menu contextual em tabela AspNetUsers no Pesquisador de objetos do SQL Server](accconfirm/_static/ssox.png)

<span data-ttu-id="9bf92-139">Observe a tabela `EmailConfirmed` campo é `False`.</span><span class="sxs-lookup"><span data-stu-id="9bf92-139">Note the table's `EmailConfirmed` field is `False`.</span></span>

<span data-ttu-id="9bf92-140">Você talvez queira usar este email novamente na próxima etapa quando o aplicativo envia um email de confirmação.</span><span class="sxs-lookup"><span data-stu-id="9bf92-140">You might want to use this email again in the next step when the app sends a confirmation email.</span></span> <span data-ttu-id="9bf92-141">Clique com botão direito na linha e selecione **excluir**.</span><span class="sxs-lookup"><span data-stu-id="9bf92-141">Right-click on the row and select **Delete**.</span></span> <span data-ttu-id="9bf92-142">Excluir o alias de email torna mais fácil nas etapas a seguir.</span><span class="sxs-lookup"><span data-stu-id="9bf92-142">Deleting the email alias makes it easier in the following steps.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="9bf92-143">CLI do .NET Core</span><span class="sxs-lookup"><span data-stu-id="9bf92-143">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="9bf92-144">Ver [trabalhar com SQLite em um projeto do ASP.NET Core MVC](xref:tutorials/first-mvc-app-xplat/working-with-sql) para obter instruções sobre como exibir o banco de dados SQLite.</span><span class="sxs-lookup"><span data-stu-id="9bf92-144">See [Work with SQLite in an ASP.NET Core MVC project](xref:tutorials/first-mvc-app-xplat/working-with-sql) for instructions on how to view the SQLite database.</span></span>

------

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a><span data-ttu-id="9bf92-145">Exigir email de confirmação</span><span class="sxs-lookup"><span data-stu-id="9bf92-145">Require email confirmation</span></span>

<span data-ttu-id="9bf92-146">É uma prática recomendada para confirmar o email de um novo registro de usuário.</span><span class="sxs-lookup"><span data-stu-id="9bf92-146">It's a best practice to confirm the email of a new user registration.</span></span> <span data-ttu-id="9bf92-147">Ajuda a confirmação para verificar se eles não estiver representando alguém de email (ou seja, eles ainda não registrados com o email de outra pessoa).</span><span class="sxs-lookup"><span data-stu-id="9bf92-147">Email confirmation helps to verify they're not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="9bf92-148">Suponha que você tinha um fórum de discussão e quisesse impedir "yli@example.com"de registrar-se como"nolivetto@contoso.com".</span><span class="sxs-lookup"><span data-stu-id="9bf92-148">Suppose you had a discussion forum, and you wanted to prevent "yli@example.com" from registering as "nolivetto@contoso.com".</span></span> <span data-ttu-id="9bf92-149">Sem email de confirmação "nolivetto@contoso.com" pode receber emails indesejados de seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="9bf92-149">Without email confirmation, "nolivetto@contoso.com" could receive unwanted email from your app.</span></span> <span data-ttu-id="9bf92-150">Suponha que o usuário registrado acidentalmente como "ylo@example.com" e ainda não tenha notado o erro de ortografia de "yli".</span><span class="sxs-lookup"><span data-stu-id="9bf92-150">Suppose the user accidentally registered as "ylo@example.com" and hadn't noticed the misspelling of "yli".</span></span> <span data-ttu-id="9bf92-151">Eles não seria capazes de usar a recuperação de senha porque o aplicativo não tiver seu email correto.</span><span class="sxs-lookup"><span data-stu-id="9bf92-151">They wouldn't be able to use password recovery because the app doesn't have their correct email.</span></span> <span data-ttu-id="9bf92-152">Email de confirmação oferece proteção limitada contra bots.</span><span class="sxs-lookup"><span data-stu-id="9bf92-152">Email confirmation provides limited protection from bots.</span></span> <span data-ttu-id="9bf92-153">Email de confirmação não fornece proteção contra usuários mal-intencionados com muitas contas de email.</span><span class="sxs-lookup"><span data-stu-id="9bf92-153">Email confirmation doesn't provide protection from malicious users with many email accounts.</span></span>

<span data-ttu-id="9bf92-154">Geralmente você deseja impedir que novos usuários incluam dados em seu site até que eles tenham um email confirmado.</span><span class="sxs-lookup"><span data-stu-id="9bf92-154">You generally want to prevent new users from posting any data to your web site before they have a confirmed email.</span></span>

<span data-ttu-id="9bf92-155">Atualização *Areas/Identity/IdentityHostingStartup.cs* para exigir um email confirmado:</span><span class="sxs-lookup"><span data-stu-id="9bf92-155">Update *Areas/Identity/IdentityHostingStartup.cs*  to require a confirmed email:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover21/Areas/Identity/IdentityHostingStartup.cs?name=snippet1&highlight=10-13)]

<span data-ttu-id="9bf92-156">`config.SignIn.RequireConfirmedEmail = true;` impede que os usuários registrados entrar até que o email seja confirmado.</span><span class="sxs-lookup"><span data-stu-id="9bf92-156">`config.SignIn.RequireConfirmedEmail = true;` prevents registered users from logging in until their email is confirmed.</span></span>

### <a name="configure-email-provider"></a><span data-ttu-id="9bf92-157">Configurar o provedor de email</span><span class="sxs-lookup"><span data-stu-id="9bf92-157">Configure email provider</span></span>

<span data-ttu-id="9bf92-158">Neste tutorial [SendGrid](https://sendgrid.com) é usado para enviar email.</span><span class="sxs-lookup"><span data-stu-id="9bf92-158">In this tutorial, [SendGrid](https://sendgrid.com) is used to send email.</span></span> <span data-ttu-id="9bf92-159">Você precisa de uma conta do SendGrid e uma chave para enviar email.</span><span class="sxs-lookup"><span data-stu-id="9bf92-159">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="9bf92-160">Você pode usar outros provedores de email.</span><span class="sxs-lookup"><span data-stu-id="9bf92-160">You can use other email providers.</span></span> <span data-ttu-id="9bf92-161">ASP.NET Core 2.x inclui `System.Net.Mail`, que permite que você enviar um email de seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="9bf92-161">ASP.NET Core 2.x includes `System.Net.Mail`, which allows you to send email from your app.</span></span> <span data-ttu-id="9bf92-162">É recomendável que usar o SendGrid ou outro serviço de email para enviar email.</span><span class="sxs-lookup"><span data-stu-id="9bf92-162">We recommend you use SendGrid or another email service to send email.</span></span> <span data-ttu-id="9bf92-163">SMTP é difícil proteger e configurado corretamente.</span><span class="sxs-lookup"><span data-stu-id="9bf92-163">SMTP is difficult to secure and set up correctly.</span></span>

<span data-ttu-id="9bf92-164">O [padrão de opções](xref:fundamentals/configuration/options) é usado para acessar as configurações de conta e chave do usuário.</span><span class="sxs-lookup"><span data-stu-id="9bf92-164">The [Options pattern](xref:fundamentals/configuration/options) is used to access the user account and key settings.</span></span> <span data-ttu-id="9bf92-165">Para obter mais informações, consulte [configuração](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="9bf92-165">For more information, see [configuration](xref:fundamentals/configuration/index).</span></span>

<span data-ttu-id="9bf92-166">Crie uma classe para buscar a chave de email seguro.</span><span class="sxs-lookup"><span data-stu-id="9bf92-166">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="9bf92-167">Para este exemplo, crie *Services/AuthMessageSenderOptions.cs*:</span><span class="sxs-lookup"><span data-stu-id="9bf92-167">For this sample, create *Services/AuthMessageSenderOptions.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover21/Services/AuthMessageSenderOptions.cs?name=snippet1)]

#### <a name="configure-sendgrid-user-secrets"></a><span data-ttu-id="9bf92-168">Configurar segredos de usuário do SendGrid</span><span class="sxs-lookup"><span data-stu-id="9bf92-168">Configure SendGrid user secrets</span></span>

<span data-ttu-id="9bf92-169">Adicionar um único `<UserSecretsId>` de valor para o `<PropertyGroup>` elemento do arquivo de projeto:</span><span class="sxs-lookup"><span data-stu-id="9bf92-169">Add a unique `<UserSecretsId>` value to the `<PropertyGroup>` element of the project file:</span></span>

[!code-xml[](accconfirm/sample/WebPWrecover21/WebPWrecover.csproj?highlight=5)]

<span data-ttu-id="9bf92-170">Defina as `SendGridUser` e `SendGridKey` com o [ferramenta secret manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="9bf92-170">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="9bf92-171">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="9bf92-171">For example:</span></span>

```console
C:/WebAppl>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="9bf92-172">No Windows, o Secret Manager armazena os pares de chaves/valor em uma *Secrets* arquivo no `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span><span class="sxs-lookup"><span data-stu-id="9bf92-172">On Windows, Secret Manager stores keys/value pairs in a *secrets.json* file in the `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span></span>

<span data-ttu-id="9bf92-173">O conteúdo a *Secrets* arquivo não são criptografadas.</span><span class="sxs-lookup"><span data-stu-id="9bf92-173">The contents of the *secrets.json* file aren't encrypted.</span></span> <span data-ttu-id="9bf92-174">O *Secrets* arquivo é mostrado abaixo (o `SendGridKey` valor tiver sido removido.)</span><span class="sxs-lookup"><span data-stu-id="9bf92-174">The *secrets.json* file is shown below (the `SendGridKey` value has been removed.)</span></span>

 ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```

### <a name="install-sendgrid"></a><span data-ttu-id="9bf92-175">Instalar o SendGrid</span><span class="sxs-lookup"><span data-stu-id="9bf92-175">Install SendGrid</span></span>

<span data-ttu-id="9bf92-176">Este tutorial mostra como adicionar notificações de email por meio [SendGrid](https://sendgrid.com/), mas você pode enviar emails usando SMTP e outros mecanismos.</span><span class="sxs-lookup"><span data-stu-id="9bf92-176">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

<span data-ttu-id="9bf92-177">Instalar o `SendGrid` pacote do NuGet:</span><span class="sxs-lookup"><span data-stu-id="9bf92-177">Install the `SendGrid` NuGet package:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9bf92-178">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9bf92-178">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="9bf92-179">No Console do Gerenciador de pacotes, digite o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="9bf92-179">From the Package Manager Console, enter the following command:</span></span>

``` PMC
Install-Package SendGrid
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="9bf92-180">CLI do .NET Core</span><span class="sxs-lookup"><span data-stu-id="9bf92-180">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="9bf92-181">No console, digite o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="9bf92-181">From the console, enter the following command:</span></span>

```cli
dotnet add package SendGrid
```

------

<span data-ttu-id="9bf92-182">Ver [inicie gratuitamente com o SendGrid](https://sendgrid.com/free/) para se registrar para uma conta gratuita do SendGrid.</span><span class="sxs-lookup"><span data-stu-id="9bf92-182">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>
### <a name="implement-iemailsender"></a><span data-ttu-id="9bf92-183">Implementar IEmailSender</span><span class="sxs-lookup"><span data-stu-id="9bf92-183">Implement IEmailSender</span></span>

<span data-ttu-id="9bf92-184">Para implementar `IEmailSender`, crie *Services/EmailSender.cs* com um código semelhante ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="9bf92-184">To Implement `IEmailSender`, create *Services/EmailSender.cs* with code similar to the following:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover21/Services/EmailSender.cs)]

### <a name="configure-startup-to-support-email"></a><span data-ttu-id="9bf92-185">Configurar a inicialização para dar suporte a email</span><span class="sxs-lookup"><span data-stu-id="9bf92-185">Configure startup to support email</span></span>

<span data-ttu-id="9bf92-186">Adicione o seguinte código para o `ConfigureServices` método na *Startup.cs* arquivo:</span><span class="sxs-lookup"><span data-stu-id="9bf92-186">Add the following code to the `ConfigureServices` method in the *Startup.cs* file:</span></span>

* <span data-ttu-id="9bf92-187">Adicionar `EmailSender` como um serviço singleton.</span><span class="sxs-lookup"><span data-stu-id="9bf92-187">Add `EmailSender` as a singleton service.</span></span>
* <span data-ttu-id="9bf92-188">Registrar o `AuthMessageSenderOptions` instância de configuração.</span><span class="sxs-lookup"><span data-stu-id="9bf92-188">Register the `AuthMessageSenderOptions` configuration instance.</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover21/Startup.cs?name=snippet2&highlight=12-99)]

## <a name="enable-account-confirmation-and-password-recovery"></a><span data-ttu-id="9bf92-189">Habilitar a recuperação de confirmação e a senha da conta</span><span class="sxs-lookup"><span data-stu-id="9bf92-189">Enable account confirmation and password recovery</span></span>

<span data-ttu-id="9bf92-190">O modelo tem o código para recuperação de confirmação e a senha da conta.</span><span class="sxs-lookup"><span data-stu-id="9bf92-190">The template has the code for account confirmation and password recovery.</span></span> <span data-ttu-id="9bf92-191">Localizar o `OnPostAsync` método no *Areas/Identity/Pages/Account/Register.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="9bf92-191">Find the `OnPostAsync` method in *Areas/Identity/Pages/Account/Register.cshtml.cs*.</span></span>

<span data-ttu-id="9bf92-192">Evitar que usuários recém-registrados seja registrada automaticamente comentando a linha a seguir:</span><span class="sxs-lookup"><span data-stu-id="9bf92-192">Prevent newly registered users from being automatically logged on by commenting out the following line:</span></span>

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="9bf92-193">O método complete é mostrado com a linha alterada realçada:</span><span class="sxs-lookup"><span data-stu-id="9bf92-193">The complete method is shown with the changed line highlighted:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover21/Areas/Identity/Pages/Account/Register.cshtml.cs?highlight=22&name=snippet_Register)]

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="9bf92-194">Registrar, confirme se o email e redefinição de senha</span><span class="sxs-lookup"><span data-stu-id="9bf92-194">Register, confirm email, and reset password</span></span>

<span data-ttu-id="9bf92-195">Executar o aplicativo web e testar o fluxo de recuperação de senha e confirmação de conta.</span><span class="sxs-lookup"><span data-stu-id="9bf92-195">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="9bf92-196">Execute o aplicativo e registrar um novo usuário</span><span class="sxs-lookup"><span data-stu-id="9bf92-196">Run the app and register a new user</span></span>

  ![Registrar conta exibição do aplicativo da Web](accconfirm/_static/loginaccconfirm1.png)

* <span data-ttu-id="9bf92-198">Verifique seu email para o link de confirmação de conta.</span><span class="sxs-lookup"><span data-stu-id="9bf92-198">Check your email for the account confirmation link.</span></span> <span data-ttu-id="9bf92-199">Ver [depurar email](#debug) se você não receber o email.</span><span class="sxs-lookup"><span data-stu-id="9bf92-199">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="9bf92-200">Clique no link para confirmar seu email.</span><span class="sxs-lookup"><span data-stu-id="9bf92-200">Click the link to confirm your email.</span></span>
* <span data-ttu-id="9bf92-201">Faça logon com seu email e senha.</span><span class="sxs-lookup"><span data-stu-id="9bf92-201">Log in with your email and password.</span></span>
* <span data-ttu-id="9bf92-202">Faça logoff.</span><span class="sxs-lookup"><span data-stu-id="9bf92-202">Log off.</span></span>

### <a name="view-the-manage-page"></a><span data-ttu-id="9bf92-203">Exibir a página Gerenciar</span><span class="sxs-lookup"><span data-stu-id="9bf92-203">View the manage page</span></span>

<span data-ttu-id="9bf92-204">Selecione seu nome de usuário no navegador: ![janela do navegador com o nome de usuário](accconfirm/_static/un.png)</span><span class="sxs-lookup"><span data-stu-id="9bf92-204">Select your user name in the browser: ![browser window with user name](accconfirm/_static/un.png)</span></span>

<span data-ttu-id="9bf92-205">Talvez seja necessário expandir a barra de navegação para ver o nome de usuário.</span><span class="sxs-lookup"><span data-stu-id="9bf92-205">You might need to expand the navbar to see user name.</span></span>

![barra de navegação](accconfirm/_static/x.png)

<span data-ttu-id="9bf92-207">A página Gerenciar é exibida com o **perfil** guia selecionada.</span><span class="sxs-lookup"><span data-stu-id="9bf92-207">The manage page is displayed with the **Profile** tab selected.</span></span> <span data-ttu-id="9bf92-208">O **Email** mostra uma caixa de seleção que indica o email foi confirmada.</span><span class="sxs-lookup"><span data-stu-id="9bf92-208">The **Email** shows a check box indicating the email has been confirmed.</span></span>

### <a name="test-password-reset"></a><span data-ttu-id="9bf92-209">Teste a redefinição de senha</span><span class="sxs-lookup"><span data-stu-id="9bf92-209">Test password reset</span></span>

* <span data-ttu-id="9bf92-210">Se você estiver conectado, selecione **Logout**.</span><span class="sxs-lookup"><span data-stu-id="9bf92-210">If you're logged in, select **Logout**.</span></span>
* <span data-ttu-id="9bf92-211">Selecione o **login** link e selecione o **esqueceu sua senha?** link.</span><span class="sxs-lookup"><span data-stu-id="9bf92-211">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="9bf92-212">Insira o email usado para registrar a conta.</span><span class="sxs-lookup"><span data-stu-id="9bf92-212">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="9bf92-213">Um email com um link para redefinir sua senha é enviado.</span><span class="sxs-lookup"><span data-stu-id="9bf92-213">An email with a link to reset your password is sent.</span></span> <span data-ttu-id="9bf92-214">Verifique seu email e clique no link para redefinir sua senha.</span><span class="sxs-lookup"><span data-stu-id="9bf92-214">Check your email and click the link to reset your password.</span></span> <span data-ttu-id="9bf92-215">Depois que sua senha foi redefinida com êxito, você pode entrar com seu email e a nova senha.</span><span class="sxs-lookup"><span data-stu-id="9bf92-215">After your password has been successfully reset, you can log in with your email and new password.</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="9bf92-216">Depurar o email</span><span class="sxs-lookup"><span data-stu-id="9bf92-216">Debug email</span></span>

<span data-ttu-id="9bf92-217">Se você não é possível obter o trabalho de email:</span><span class="sxs-lookup"><span data-stu-id="9bf92-217">If you can't get email working:</span></span>

* <span data-ttu-id="9bf92-218">Defina um ponto de interrupção no `EmailSender.Execute` para verificar `SendGridClient.SendEmailAsync` é chamado.</span><span class="sxs-lookup"><span data-stu-id="9bf92-218">Set a breakpoint in `EmailSender.Execute` to verify `SendGridClient.SendEmailAsync` is called.</span></span>
* <span data-ttu-id="9bf92-219">Criar uma [aplicativo de console para enviar email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) usando um código semelhante ao `EmailSender.Execute`.</span><span class="sxs-lookup"><span data-stu-id="9bf92-219">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) using similar code to `EmailSender.Execute`.</span></span>
* <span data-ttu-id="9bf92-220">Examine os [atividade de Email](https://sendgrid.com/docs/User_Guide/email_activity.html) página.</span><span class="sxs-lookup"><span data-stu-id="9bf92-220">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="9bf92-221">Verifique sua pasta de spam.</span><span class="sxs-lookup"><span data-stu-id="9bf92-221">Check your spam folder.</span></span>
* <span data-ttu-id="9bf92-222">Tente outro alias de email em outro provedor de email (Microsoft, Yahoo, Gmail, etc.)</span><span class="sxs-lookup"><span data-stu-id="9bf92-222">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="9bf92-223">Tente enviar para contas de email diferente.</span><span class="sxs-lookup"><span data-stu-id="9bf92-223">Try sending to different email accounts.</span></span>

<span data-ttu-id="9bf92-224">**Uma prática recomendada de segurança** é **não** use segredos de produção em desenvolvimento e teste.</span><span class="sxs-lookup"><span data-stu-id="9bf92-224">**A security best practice** is to **not** use production secrets in test and development.</span></span> <span data-ttu-id="9bf92-225">Se você publicar o aplicativo no Azure, você pode definir os segredos do SendGrid como configurações de aplicativo no portal do aplicativo Web do Azure.</span><span class="sxs-lookup"><span data-stu-id="9bf92-225">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="9bf92-226">Configurar o sistema de configuração para ler as chaves de variáveis de ambiente.</span><span class="sxs-lookup"><span data-stu-id="9bf92-226">The configuration system is set up to read keys from environment variables.</span></span>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="9bf92-227">Combine as contas de logon social e local</span><span class="sxs-lookup"><span data-stu-id="9bf92-227">Combine social and local login accounts</span></span>

<span data-ttu-id="9bf92-228">Para concluir esta seção, você deve primeiro habilitar um provedor de autenticação externa.</span><span class="sxs-lookup"><span data-stu-id="9bf92-228">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="9bf92-229">Ver [Facebook, Google e a autenticação de provedor externo](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="9bf92-229">See [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="9bf92-230">Você pode combinar as contas locais e sociais clicando no link seu email.</span><span class="sxs-lookup"><span data-stu-id="9bf92-230">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="9bf92-231">Na sequência a seguir, "RickAndMSFT@gmail.com" é criado como um logon local; no entanto, você pode criar a conta como um logon social primeiro e adicionar um logon local.</span><span class="sxs-lookup"><span data-stu-id="9bf92-231">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Aplicativo Web: RickAndMSFT@gmail.com usuário autenticado](accconfirm/_static/rick.png)

<span data-ttu-id="9bf92-233">Clique no **gerenciar** link.</span><span class="sxs-lookup"><span data-stu-id="9bf92-233">Click on the **Manage** link.</span></span> <span data-ttu-id="9bf92-234">Observe externo 0 (logons sociais) associado a essa conta.</span><span class="sxs-lookup"><span data-stu-id="9bf92-234">Note the 0 external (social logins) associated with this account.</span></span>

![Gerenciar o modo de exibição](accconfirm/_static/manage.png)

<span data-ttu-id="9bf92-236">Clique no link para outro serviço de logon e aceitar as solicitações do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="9bf92-236">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="9bf92-237">Na imagem a seguir, o Facebook é o provedor de autenticação externa:</span><span class="sxs-lookup"><span data-stu-id="9bf92-237">In the following image, Facebook is the external authentication provider:</span></span>

![Gerenciar seu modo de exibição de logons externos lista Facebook](accconfirm/_static/fb.png)

<span data-ttu-id="9bf92-239">As duas contas foram combinadas.</span><span class="sxs-lookup"><span data-stu-id="9bf92-239">The two accounts have been combined.</span></span> <span data-ttu-id="9bf92-240">É possível fazer logon com qualquer uma das contas.</span><span class="sxs-lookup"><span data-stu-id="9bf92-240">You are able to log on with either account.</span></span> <span data-ttu-id="9bf92-241">Convém que os usuários adicionem contas locais no caso de seu serviço de autenticação de logon social está inoperante ou, mais provavelmente eles tiver perdido o acesso à sua conta social.</span><span class="sxs-lookup"><span data-stu-id="9bf92-241">You might want your users to add local accounts in case their social login authentication service is down, or more likely they've lost access to their social account.</span></span>

## <a name="enable-account-confirmation-after-a-site-has-users"></a><span data-ttu-id="9bf92-242">Habilitar confirmação de conta depois que um site tem usuários</span><span class="sxs-lookup"><span data-stu-id="9bf92-242">Enable account confirmation after a site has users</span></span>

<span data-ttu-id="9bf92-243">Habilitando a confirmação de conta em um site com usuários bloqueia todos os usuários existentes.</span><span class="sxs-lookup"><span data-stu-id="9bf92-243">Enabling account confirmation on a site with users locks out all the existing users.</span></span> <span data-ttu-id="9bf92-244">Os usuários existentes estão bloqueados porque suas contas não são confirmadas.</span><span class="sxs-lookup"><span data-stu-id="9bf92-244">Existing users are locked out because their accounts aren't confirmed.</span></span> <span data-ttu-id="9bf92-245">Para contornar o bloqueio de usuário existente, use uma das seguintes abordagens:</span><span class="sxs-lookup"><span data-stu-id="9bf92-245">To work around existing user lockout, use one of the following approaches:</span></span>

* <span data-ttu-id="9bf92-246">Atualize o banco de dados para marcar todos os usuários existentes como sendo confirmada.</span><span class="sxs-lookup"><span data-stu-id="9bf92-246">Update the database to mark all existing users as being confirmed.</span></span>
* <span data-ttu-id="9bf92-247">Confirme se os usuários existentes.</span><span class="sxs-lookup"><span data-stu-id="9bf92-247">Confirm exiting users.</span></span> <span data-ttu-id="9bf92-248">Por exemplo, envio em lote-emails com links de confirmação.</span><span class="sxs-lookup"><span data-stu-id="9bf92-248">For example, batch-send emails with confirmation links.</span></span>

::: moniker-end
