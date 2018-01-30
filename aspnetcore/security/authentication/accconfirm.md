---
title: "Confirmação de conta e senha de recuperação no núcleo do ASP.NET"
author: rick-anderson
description: "Mostra como criar um aplicativo do ASP.NET Core com redefinição de senha e de confirmação de email."
manager: wpickett
ms.author: riande
ms.date: 12/1/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/accconfirm
ms.openlocfilehash: 459f1793b1f1f73792bb6537856cb739774c6261
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2018
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a><span data-ttu-id="c9e11-103">Confirmação de conta e de recuperação de senha no núcleo do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c9e11-103">Account confirmation and password recovery in ASP.NET Core</span></span>

<span data-ttu-id="c9e11-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT) e [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="c9e11-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span> 

<span data-ttu-id="c9e11-105">Este tutorial mostra como criar um aplicativo do ASP.NET Core com redefinição de senha e de confirmação de email.</span><span class="sxs-lookup"><span data-stu-id="c9e11-105">This tutorial shows you how to build an ASP.NET Core app with email confirmation and password reset.</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="c9e11-106">Criar um novo projeto ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c9e11-106">Create a New ASP.NET Core Project</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c9e11-107">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c9e11-107">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="c9e11-108">Essa etapa se aplica ao Visual Studio no Windows.</span><span class="sxs-lookup"><span data-stu-id="c9e11-108">This step applies to Visual Studio on Windows.</span></span> <span data-ttu-id="c9e11-109">Consulte a próxima seção para obter instruções de CLI.</span><span class="sxs-lookup"><span data-stu-id="c9e11-109">See the next section for CLI instructions.</span></span>

<span data-ttu-id="c9e11-110">O tutorial requer o Visual Studio 2017 Preview 2 ou posterior.</span><span class="sxs-lookup"><span data-stu-id="c9e11-110">The tutorial requires Visual Studio 2017 Preview 2 or later.</span></span>

* <span data-ttu-id="c9e11-111">No Visual Studio, crie um novo projeto de aplicativo Web.</span><span class="sxs-lookup"><span data-stu-id="c9e11-111">In Visual Studio, create a New Web Application Project.</span></span>
* <span data-ttu-id="c9e11-112">Selecione **Core ASP.NET 2.0**.</span><span class="sxs-lookup"><span data-stu-id="c9e11-112">Select **ASP.NET Core 2.0**.</span></span> <span data-ttu-id="c9e11-113">A imagem a seguir mostram **.NET Core** selecionado, mas você pode selecionar **do .NET Framework**.</span><span class="sxs-lookup"><span data-stu-id="c9e11-113">The following image show **.NET Core** selected, but you can select **.NET Framework**.</span></span>
* <span data-ttu-id="c9e11-114">Selecione **alterar autenticação** e definido como **contas de usuário individuais**.</span><span class="sxs-lookup"><span data-stu-id="c9e11-114">Select **Change Authentication** and set to **Individual User Accounts**.</span></span>
* <span data-ttu-id="c9e11-115">Mantenha o padrão **no aplicativo de contas de usuário do repositório**.</span><span class="sxs-lookup"><span data-stu-id="c9e11-115">Keep the default **Store user accounts in-app**.</span></span>

![Nova caixa de diálogo do projeto mostrando "Opção de contas de usuário individuais" selecionada](accconfirm/_static/2.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c9e11-117">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c9e11-117">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="c9e11-118">O tutorial requer o Visual Studio 2017 ou posterior.</span><span class="sxs-lookup"><span data-stu-id="c9e11-118">The tutorial requires Visual Studio 2017 or later.</span></span>

* <span data-ttu-id="c9e11-119">No Visual Studio, crie um novo projeto de aplicativo Web.</span><span class="sxs-lookup"><span data-stu-id="c9e11-119">In Visual Studio, create a New Web Application Project.</span></span>
* <span data-ttu-id="c9e11-120">Selecione **alterar autenticação** e definido como **contas de usuário individuais**.</span><span class="sxs-lookup"><span data-stu-id="c9e11-120">Select **Change Authentication** and set to **Individual User Accounts**.</span></span>

![Nova caixa de diálogo do projeto mostrando "Opção de contas de usuário individuais" selecionada](accconfirm/_static/indiv.png)

---

### <a name="net-core-cli-project-creation-for-macos-and-linux"></a><span data-ttu-id="c9e11-122">Criação do projeto .NET core CLI para macOS e Linux</span><span class="sxs-lookup"><span data-stu-id="c9e11-122">.NET Core CLI project creation for macOS and Linux</span></span>

<span data-ttu-id="c9e11-123">Se você estiver usando a CLI ou SQLite, execute o seguinte em uma janela de comando:</span><span class="sxs-lookup"><span data-stu-id="c9e11-123">If you're using the CLI or SQLite, run the following in a command window:</span></span>

```console
dotnet new mvc --auth Individual
```

* <span data-ttu-id="c9e11-124">`--auth Individual`Especifica o modelo de contas de usuário individuais.</span><span class="sxs-lookup"><span data-stu-id="c9e11-124">`--auth Individual` specifies the Individual User Accounts template.</span></span>
* <span data-ttu-id="c9e11-125">No Windows, adicione o `-uld` opção.</span><span class="sxs-lookup"><span data-stu-id="c9e11-125">On Windows, add the `-uld` option.</span></span> <span data-ttu-id="c9e11-126">O `-uld` opção cria uma cadeia de caracteres de conexão do LocalDB em vez de um banco de dados SQLite.</span><span class="sxs-lookup"><span data-stu-id="c9e11-126">The `-uld` option creates a LocalDB connection string rather than a SQLite DB.</span></span>
* <span data-ttu-id="c9e11-127">Execute `new mvc --help` para obter ajuda sobre este comando.</span><span class="sxs-lookup"><span data-stu-id="c9e11-127">Run `new mvc --help` to get help on this command.</span></span>

## <a name="test-new-user-registration"></a><span data-ttu-id="c9e11-128">Testar o novo registro de usuário</span><span class="sxs-lookup"><span data-stu-id="c9e11-128">Test new user registration</span></span>

<span data-ttu-id="c9e11-129">Executar o aplicativo, selecione o **registrar** vincular e registrar um usuário.</span><span class="sxs-lookup"><span data-stu-id="c9e11-129">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="c9e11-130">Siga as instruções para executar migrações de Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="c9e11-130">Follow the instructions to run Entity Framework Core migrations.</span></span> <span data-ttu-id="c9e11-131">Neste ponto, a validação somente do email é com o [[EmailAddress]](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) atributo.</span><span class="sxs-lookup"><span data-stu-id="c9e11-131">At this  point, the only validation on the email is with the [[EmailAddress]](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attribute.</span></span> <span data-ttu-id="c9e11-132">Depois de enviar o registro, você está conectado no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="c9e11-132">After you submit the registration, you are logged into the app.</span></span> <span data-ttu-id="c9e11-133">Posteriormente no tutorial, vamos alterar isso para que novos usuários não podem fazer logon até que o email foi validado.</span><span class="sxs-lookup"><span data-stu-id="c9e11-133">Later in the tutorial, we'll change this so new users cannot log in until their email has been validated.</span></span>

## <a name="view-the-identity-database"></a><span data-ttu-id="c9e11-134">Exibir o banco de dados de identidade</span><span class="sxs-lookup"><span data-stu-id="c9e11-134">View the Identity database</span></span>

# <a name="sql-servertabsql-server"></a>[<span data-ttu-id="c9e11-135">SQL Server</span><span class="sxs-lookup"><span data-stu-id="c9e11-135">SQL Server</span></span>](#tab/sql-server)

* <span data-ttu-id="c9e11-136">Do **exibição** menu, selecione **Pesquisador de objetos do SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="c9e11-136">From the **View** menu, select **SQL Server Object Explorer** (SSOX).</span></span> 
* <span data-ttu-id="c9e11-137">Navegue até **(localdb) MSSQLLocalDB (SQL Server 13)**.</span><span class="sxs-lookup"><span data-stu-id="c9e11-137">Navigate to **(localdb)MSSQLLocalDB(SQL Server 13)**.</span></span> <span data-ttu-id="c9e11-138">Clique duas vezes em **dbo. AspNetUsers** > **exibir dados**:</span><span class="sxs-lookup"><span data-stu-id="c9e11-138">Right-click on **dbo.AspNetUsers** > **View Data**:</span></span>

![Menu de contexto no AspNetUsers tabela no Pesquisador de objetos do SQL Server](accconfirm/_static/ssox.png)

<span data-ttu-id="c9e11-140">Observe o `EmailConfirmed` campo é `False`.</span><span class="sxs-lookup"><span data-stu-id="c9e11-140">Note the `EmailConfirmed` field is `False`.</span></span>

<span data-ttu-id="c9e11-141">Você talvez queira usar o email novamente na próxima etapa, quando o aplicativo envia um email de confirmação.</span><span class="sxs-lookup"><span data-stu-id="c9e11-141">You might want to use this email again in the next step when the app sends a confirmation email.</span></span> <span data-ttu-id="c9e11-142">Clique na linha e selecione **excluir**.</span><span class="sxs-lookup"><span data-stu-id="c9e11-142">Right-click on the row and select **Delete**.</span></span> <span data-ttu-id="c9e11-143">Excluir o email alias agora facilitará nas etapas a seguir.</span><span class="sxs-lookup"><span data-stu-id="c9e11-143">Deleting the email alias now will make it easier in the following steps.</span></span>

# <a name="sqlitetabsqlite"></a>[<span data-ttu-id="c9e11-144">SQLite</span><span class="sxs-lookup"><span data-stu-id="c9e11-144">SQLite</span></span>](#tab/sqlite)

<span data-ttu-id="c9e11-145">Consulte [trabalhando com SQLite em um projeto MVC do ASP.NET Core](xref:tutorials/first-mvc-app-xplat/working-with-sql) para obter instruções sobre como exibir o banco de dados SQLite.</span><span class="sxs-lookup"><span data-stu-id="c9e11-145">See [Working with SQLite in an ASP.NET Core MVC project](xref:tutorials/first-mvc-app-xplat/working-with-sql) for instructions on how to view the SQLite DB.</span></span> 

---

## <a name="require-ssl-and-setup-iis-express-for-ssl"></a><span data-ttu-id="c9e11-146">Exigir SSL e configurar o IIS Express para SSL</span><span class="sxs-lookup"><span data-stu-id="c9e11-146">Require SSL and setup IIS Express for SSL</span></span>

<span data-ttu-id="c9e11-147">Consulte [impondo SSL](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="c9e11-147">See [Enforcing SSL](xref:security/enforcing-ssl).</span></span>

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a><span data-ttu-id="c9e11-148">Solicitar confirmação de email</span><span class="sxs-lookup"><span data-stu-id="c9e11-148">Require email confirmation</span></span>

<span data-ttu-id="c9e11-149">É uma prática recomendada para confirmar o email de um novo registro de usuário para verificar se eles não estiver representando outra pessoa (ou seja, eles ainda não registrados com outra pessoa email).</span><span class="sxs-lookup"><span data-stu-id="c9e11-149">It's a best practice to confirm the email of a new user registration to verify they're not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="c9e11-150">Suponha que você tivesse um fórum de discussão, e quiser impedir "yli@example.com"do registro como"nolivetto@contoso.com."</span><span class="sxs-lookup"><span data-stu-id="c9e11-150">Suppose you had a discussion forum, and you wanted to prevent "yli@example.com" from registering as "nolivetto@contoso.com."</span></span> <span data-ttu-id="c9e11-151">Sem confirmação por email, "nolivetto@contoso.com" poderia obter indesejadas de seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="c9e11-151">Without email confirmation, "nolivetto@contoso.com" could get unwanted email from your app.</span></span> <span data-ttu-id="c9e11-152">Suponha que o usuário registrado acidentalmente como "ylo@example.com" e ainda não tenha notado o erro de "yli", elas não serão capazes de usar a recuperação de senha porque o aplicativo não tiver seu email correto.</span><span class="sxs-lookup"><span data-stu-id="c9e11-152">Suppose the user accidentally registered as "ylo@example.com" and hadn't noticed the misspelling of "yli," they wouldn't be able to use password recovery because the app doesn't have their correct email.</span></span> <span data-ttu-id="c9e11-153">Email de confirmação oferece apenas proteção limitada de robôs e não oferece proteção contra spam determinado que têm muitos aliases de email de trabalho, que eles podem usar para registrar.</span><span class="sxs-lookup"><span data-stu-id="c9e11-153">Email confirmation provides only limited protection from bots and doesn't provide protection from determined spammers who have many working email aliases they can use to register.</span></span>

<span data-ttu-id="c9e11-154">Em geral você deseja impedir que novos usuários lançamento todos os dados para seu site da web para que eles tenham um email confirmado.</span><span class="sxs-lookup"><span data-stu-id="c9e11-154">You generally want to prevent new users from posting any data to your web site before they have a confirmed email.</span></span> 

<span data-ttu-id="c9e11-155">Atualização `ConfigureServices` para exigir um email confirmado:</span><span class="sxs-lookup"><span data-stu-id="c9e11-155">Update `ConfigureServices` to require a confirmed email:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c9e11-156">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c9e11-156">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](accconfirm/sample/WebPW/Startup.cs?name=snippet1&highlight=6-9)]


# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c9e11-157">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c9e11-157">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=13-16)]

---

 
```csharp
config.SignIn.RequireConfirmedEmail = true;
```
<span data-ttu-id="c9e11-158">A linha precedente impede que usuários registrados que está sendo conectado até que o email foi confirmado.</span><span class="sxs-lookup"><span data-stu-id="c9e11-158">The preceding line prevents registered users from being logged in until their email is confirmed.</span></span> <span data-ttu-id="c9e11-159">No entanto, essa linha não impede que novos usuários que está sendo conectado depois que eles se registrar.</span><span class="sxs-lookup"><span data-stu-id="c9e11-159">However, that line doesn't prevent new users from being logged in after they register.</span></span> <span data-ttu-id="c9e11-160">O código padrão registra em um usuário depois que eles se registrar.</span><span class="sxs-lookup"><span data-stu-id="c9e11-160">The default code logs in a user after they register.</span></span> <span data-ttu-id="c9e11-161">Depois que eles fazer logoff, eles não será capazes de fazer logon novamente até que eles se registrar.</span><span class="sxs-lookup"><span data-stu-id="c9e11-161">Once they log out, they won't be able to log in again until they register.</span></span> <span data-ttu-id="c9e11-162">Posteriormente no tutorial, vamos alterar o usuário de código registrado para recentemente são **não** conectado.</span><span class="sxs-lookup"><span data-stu-id="c9e11-162">Later in the tutorial we'll change the code so newly registered user are **not** logged in.</span></span>

### <a name="configure-email-provider"></a><span data-ttu-id="c9e11-163">Configurar o provedor de email</span><span class="sxs-lookup"><span data-stu-id="c9e11-163">Configure email provider</span></span>

<span data-ttu-id="c9e11-164">Neste tutorial, SendGrid é usada para enviar email.</span><span class="sxs-lookup"><span data-stu-id="c9e11-164">In this tutorial, SendGrid is used to send email.</span></span> <span data-ttu-id="c9e11-165">Você precisa de uma conta do SendGrid e a chave para enviar email.</span><span class="sxs-lookup"><span data-stu-id="c9e11-165">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="c9e11-166">Você pode usar outros provedores de email.</span><span class="sxs-lookup"><span data-stu-id="c9e11-166">You can use other email providers.</span></span> <span data-ttu-id="c9e11-167">ASP.NET Core 2. x inclui `System.Net.Mail`, que permite enviar email de seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="c9e11-167">ASP.NET Core 2.x includes `System.Net.Mail`, which allows you to send email from your app.</span></span> <span data-ttu-id="c9e11-168">É recomendável que usar o SendGrid ou outro serviço de email para enviar email.</span><span class="sxs-lookup"><span data-stu-id="c9e11-168">We recommend you use SendGrid or another email service to send email.</span></span>

<span data-ttu-id="c9e11-169">O [padrão de opções](xref:fundamentals/configuration/options) é usado para acessar as configurações de conta e a chave de usuário.</span><span class="sxs-lookup"><span data-stu-id="c9e11-169">The [Options pattern](xref:fundamentals/configuration/options) is used to access the user account and key settings.</span></span> <span data-ttu-id="c9e11-170">Para obter mais informações, consulte [configuração](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="c9e11-170">For more information, see [configuration](xref:fundamentals/configuration/index).</span></span>

<span data-ttu-id="c9e11-171">Crie uma classe para obter a chave de email seguro.</span><span class="sxs-lookup"><span data-stu-id="c9e11-171">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="c9e11-172">Para este exemplo, o `AuthMessageSenderOptions` classe é criada no *Services/AuthMessageSenderOptions.cs* arquivo.</span><span class="sxs-lookup"><span data-stu-id="c9e11-172">For this sample, the `AuthMessageSenderOptions` class is created in the *Services/AuthMessageSenderOptions.cs* file.</span></span>

[!code-csharp[Main](accconfirm/sample/WebApp1/Services/AuthMessageSenderOptions.cs?name=snippet1)]

<span data-ttu-id="c9e11-173">Definir o `SendGridUser` e `SendGridKey` com o [ferramenta Gerenciador de segredo](../app-secrets.md).</span><span class="sxs-lookup"><span data-stu-id="c9e11-173">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](../app-secrets.md).</span></span> <span data-ttu-id="c9e11-174">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="c9e11-174">For example:</span></span>

```none
C:\WebAppl\src\WebApp1>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="c9e11-175">No Windows, o segredo Manager armazena seus pares de chaves/valor em uma *secrets.json* arquivo no diretório %APPDATA%/Microsoft/UserSecrets/ < WebAppName userSecretsId >.</span><span class="sxs-lookup"><span data-stu-id="c9e11-175">On Windows, Secret Manager stores your keys/value pairs in a *secrets.json* file in the %APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId> directory.</span></span>

<span data-ttu-id="c9e11-176">O conteúdo do *secrets.json* arquivo não são criptografados.</span><span class="sxs-lookup"><span data-stu-id="c9e11-176">The contents of the *secrets.json* file are not encrypted.</span></span> <span data-ttu-id="c9e11-177">O *secrets.json* arquivo é mostrado a seguir (o `SendGridKey` valor foi removido.)</span><span class="sxs-lookup"><span data-stu-id="c9e11-177">The *secrets.json* file is shown below (the `SendGridKey` value has been removed.)</span></span>

  ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```

### <a name="configure-startup-to-use-authmessagesenderoptions"></a><span data-ttu-id="c9e11-178">Configurar a inicialização para usar AuthMessageSenderOptions</span><span class="sxs-lookup"><span data-stu-id="c9e11-178">Configure startup to use AuthMessageSenderOptions</span></span>

<span data-ttu-id="c9e11-179">Adicionar `AuthMessageSenderOptions` ao contêiner de serviço no final o `ConfigureServices` método o *Startup.cs* arquivo:</span><span class="sxs-lookup"><span data-stu-id="c9e11-179">Add `AuthMessageSenderOptions` to the service container at the end of the `ConfigureServices` method in the *Startup.cs* file:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c9e11-180">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c9e11-180">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](accconfirm/sample/WebPW/Startup.cs?name=snippet1&highlight=18)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c9e11-181">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c9e11-181">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
[!code-csharp[Main](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=26)]

---

### <a name="configure-the-authmessagesender-class"></a><span data-ttu-id="c9e11-182">Configurar a classe AuthMessageSender</span><span class="sxs-lookup"><span data-stu-id="c9e11-182">Configure the AuthMessageSender class</span></span>

<span data-ttu-id="c9e11-183">Este tutorial mostra como adicionar notificações de email por meio de [SendGrid](https://sendgrid.com/), mas você pode enviar emails usando SMTP e outros mecanismos.</span><span class="sxs-lookup"><span data-stu-id="c9e11-183">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

* <span data-ttu-id="c9e11-184">Instalar o `SendGrid` pacote NuGet.</span><span class="sxs-lookup"><span data-stu-id="c9e11-184">Install the `SendGrid` NuGet package.</span></span> <span data-ttu-id="c9e11-185">No Console do Gerenciador de pacotes, digite o seguinte comando a seguir:</span><span class="sxs-lookup"><span data-stu-id="c9e11-185">From the Package Manager Console,  enter the following the following command:</span></span>

  `Install-Package SendGrid`

* <span data-ttu-id="c9e11-186">Consulte [comece com SendGrid gratuitamente](https://sendgrid.com/free/) para registrar-se para uma conta gratuita do SendGrid.</span><span class="sxs-lookup"><span data-stu-id="c9e11-186">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>

#### <a name="configure-sendgrid"></a><span data-ttu-id="c9e11-187">Configurar o SendGrid</span><span class="sxs-lookup"><span data-stu-id="c9e11-187">Configure SendGrid</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c9e11-188">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c9e11-188">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

* <span data-ttu-id="c9e11-189">Adicione código *Services/EmailSender.cs* semelhante à seguinte para configurar o SendGrid:</span><span class="sxs-lookup"><span data-stu-id="c9e11-189">Add code in *Services/EmailSender.cs* similar to the following to configure SendGrid:</span></span>

[!code-csharp[Main](accconfirm/sample/WebPW/Services/EmailSender.cs)]


# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c9e11-190">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c9e11-190">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
* <span data-ttu-id="c9e11-191">Adicione código *Services/MessageServices.cs* semelhante à seguinte para configurar o SendGrid:</span><span class="sxs-lookup"><span data-stu-id="c9e11-191">Add code in *Services/MessageServices.cs* similar to the following to configure SendGrid:</span></span>

[!code-csharp[Main](accconfirm/sample/WebApp1/Services/MessageServices.cs)]

---

## <a name="enable-account-confirmation-and-password-recovery"></a><span data-ttu-id="c9e11-192">Habilitar a recuperação de confirmação e a senha da conta</span><span class="sxs-lookup"><span data-stu-id="c9e11-192">Enable account confirmation and password recovery</span></span>

<span data-ttu-id="c9e11-193">O modelo tem o código de recuperação de confirmação e a senha da conta.</span><span class="sxs-lookup"><span data-stu-id="c9e11-193">The template has the code for account confirmation and password recovery.</span></span> <span data-ttu-id="c9e11-194">Localizar o `[HttpPost] Register` método o *AccountController.cs* arquivo.</span><span class="sxs-lookup"><span data-stu-id="c9e11-194">Find the `[HttpPost] Register` method in the  *AccountController.cs* file.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c9e11-195">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c9e11-195">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="c9e11-196">Impedi que usuários recém-registrados sendo registrados automaticamente pelo comentar a seguinte linha:</span><span class="sxs-lookup"><span data-stu-id="c9e11-196">Prevent newly registered users from being automatically logged on by commenting out the following line:</span></span>

```csharp 
await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="c9e11-197">O método complete é mostrado com a linha alterada realçada:</span><span class="sxs-lookup"><span data-stu-id="c9e11-197">The complete method is shown with the changed line highlighted:</span></span>

[!code-csharp[Main](accconfirm/sample/WebPW/Controllers/AccountController.cs?highlight=19&name=snippet_Register)]

<span data-ttu-id="c9e11-198">Observação: O código anterior falhará se você implementar `IEmailSender` e enviar um email de texto sem formatação.</span><span class="sxs-lookup"><span data-stu-id="c9e11-198">Note: The previous code will fail if you implement `IEmailSender` and send a plain text email.</span></span> <span data-ttu-id="c9e11-199">Consulte [esse problema](https://github.com/aspnet/Home/issues/2152) para obter mais informações e uma solução alternativa.</span><span class="sxs-lookup"><span data-stu-id="c9e11-199">See [this issue](https://github.com/aspnet/Home/issues/2152) for more information and a workaround.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c9e11-200">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c9e11-200">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="c9e11-201">Descomente o código para habilitar a confirmação da conta.</span><span class="sxs-lookup"><span data-stu-id="c9e11-201">Uncomment the code to enable account confirmation.</span></span>

[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=16-25&name=snippet_Register)]

<span data-ttu-id="c9e11-202">Observação: É estiver também impedindo que um usuário registrado recentemente sendo registrados automaticamente pelo comentar a seguinte linha:</span><span class="sxs-lookup"><span data-stu-id="c9e11-202">Note: We're also preventing a newly-registered user from being automatically logged on by commenting out the following line:</span></span>

```csharp 
//await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="c9e11-203">Habilitar a recuperação de senha por uncommenting o código de `ForgotPassword` ação no *Controllers/AccountController.cs* arquivo.</span><span class="sxs-lookup"><span data-stu-id="c9e11-203">Enable password recovery by uncommenting the code in the `ForgotPassword` action in the *Controllers/AccountController.cs* file.</span></span>

[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=17-23&name=snippet_ForgotPassword)]

<span data-ttu-id="c9e11-204">Remova o elemento de formulário em *Views/Account/ForgotPassword.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="c9e11-204">Uncomment the form element in *Views/Account/ForgotPassword.cshtml*.</span></span> <span data-ttu-id="c9e11-205">Talvez você queira remover o `<p> For more information on how to enable reset password ... </p>` elemento que contém um link para este artigo.</span><span class="sxs-lookup"><span data-stu-id="c9e11-205">You might want to remove the `<p> For more information on how to enable reset password ... </p>` element which contains a link to this article.</span></span>

[!code-html[Main](accconfirm/sample/WebApp1/Views/Account/ForgotPassword.cshtml?highlight=7-10,12,28)]

---

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="c9e11-206">Registrar, confirme o email e redefinição de senha</span><span class="sxs-lookup"><span data-stu-id="c9e11-206">Register, confirm email, and reset password</span></span>

<span data-ttu-id="c9e11-207">Executar o aplicativo web e testar o fluxo de recuperação de senha e confirmação de conta.</span><span class="sxs-lookup"><span data-stu-id="c9e11-207">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="c9e11-208">Execute o aplicativo e registrar um novo usuário</span><span class="sxs-lookup"><span data-stu-id="c9e11-208">Run the app and register a new user</span></span>

 ![Registrar conta exibição do aplicativo Web](accconfirm/_static/loginaccconfirm1.png)

* <span data-ttu-id="c9e11-210">Verifique seu email para o link de confirmação de conta.</span><span class="sxs-lookup"><span data-stu-id="c9e11-210">Check your email for the account confirmation link.</span></span> <span data-ttu-id="c9e11-211">Consulte [depurar email](#debug) se você não receber o email.</span><span class="sxs-lookup"><span data-stu-id="c9e11-211">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="c9e11-212">Clique no link para confirmar seu email.</span><span class="sxs-lookup"><span data-stu-id="c9e11-212">Click the link to confirm your email.</span></span>
* <span data-ttu-id="c9e11-213">Faça logon com seu email e senha.</span><span class="sxs-lookup"><span data-stu-id="c9e11-213">Log in with your email and password.</span></span>
* <span data-ttu-id="c9e11-214">Fazer logoff.</span><span class="sxs-lookup"><span data-stu-id="c9e11-214">Log off.</span></span>

### <a name="view-the-manage-page"></a><span data-ttu-id="c9e11-215">Exibir a página de gerenciamento</span><span class="sxs-lookup"><span data-stu-id="c9e11-215">View the manage page</span></span>

<span data-ttu-id="c9e11-216">Selecione o nome de usuário no navegador: ![janela do navegador com o nome de usuário](accconfirm/_static/un.png)</span><span class="sxs-lookup"><span data-stu-id="c9e11-216">Select your user name in the browser: ![browser window with user name](accconfirm/_static/un.png)</span></span>

<span data-ttu-id="c9e11-217">Talvez seja necessário expandir a barra de navegação para ver o nome de usuário.</span><span class="sxs-lookup"><span data-stu-id="c9e11-217">You might need to expand the navbar to see user name.</span></span>

![navbar](accconfirm/_static/x.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c9e11-219">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c9e11-219">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="c9e11-220">A página de gerenciamento é exibida com o **perfil** guia selecionada.</span><span class="sxs-lookup"><span data-stu-id="c9e11-220">The manage page is displayed with the **Profile** tab selected.</span></span> <span data-ttu-id="c9e11-221">O **Email** mostra uma caixa de seleção que indica o email foi confirmada.</span><span class="sxs-lookup"><span data-stu-id="c9e11-221">The **Email** shows a check box indicating the email has been confirmed.</span></span> 

![página Gerenciar](accconfirm/_static/rick2.png)


# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c9e11-223">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c9e11-223">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="c9e11-224">Falaremos sobre esta página no tutorial posteriormente.</span><span class="sxs-lookup"><span data-stu-id="c9e11-224">We'll talk about this page later in the tutorial.</span></span>
<span data-ttu-id="c9e11-225">![página Gerenciar](accconfirm/_static/rick2.png)</span><span class="sxs-lookup"><span data-stu-id="c9e11-225">![manage page](accconfirm/_static/rick2.png)</span></span>

---

### <a name="test-password-reset"></a><span data-ttu-id="c9e11-226">Redefinição de senha do teste</span><span class="sxs-lookup"><span data-stu-id="c9e11-226">Test password reset</span></span>

* <span data-ttu-id="c9e11-227">Se você estiver conectado, selecione **Logout**.</span><span class="sxs-lookup"><span data-stu-id="c9e11-227">If you're logged in, select **Logout**.</span></span>  
* <span data-ttu-id="c9e11-228">Selecione o **login** link e selecione o **esqueceu sua senha?** link.</span><span class="sxs-lookup"><span data-stu-id="c9e11-228">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="c9e11-229">Insira o email que é usado para registrar a conta.</span><span class="sxs-lookup"><span data-stu-id="c9e11-229">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="c9e11-230">Será enviado um email com um link para redefinir sua senha.</span><span class="sxs-lookup"><span data-stu-id="c9e11-230">An email with a link to reset your password will be sent.</span></span> <span data-ttu-id="c9e11-231">Verifique seu email e clique no link para redefinir sua senha.</span><span class="sxs-lookup"><span data-stu-id="c9e11-231">Check your email and click the link to reset your password.</span></span>  <span data-ttu-id="c9e11-232">Depois que sua senha foi redefinida com êxito, você pode fazer logon com seu email e a nova senha.</span><span class="sxs-lookup"><span data-stu-id="c9e11-232">After your password has been successfully reset, you can login with your email and new password.</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="c9e11-233">Depurar email</span><span class="sxs-lookup"><span data-stu-id="c9e11-233">Debug email</span></span>

<span data-ttu-id="c9e11-234">Se você não pode receber email de trabalho:</span><span class="sxs-lookup"><span data-stu-id="c9e11-234">If you can't get email working:</span></span>

* <span data-ttu-id="c9e11-235">Examine o [atividade Email](https://sendgrid.com/docs/User_Guide/email_activity.html) página.</span><span class="sxs-lookup"><span data-stu-id="c9e11-235">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="c9e11-236">Verifique a pasta de spam.</span><span class="sxs-lookup"><span data-stu-id="c9e11-236">Check your spam folder.</span></span>
* <span data-ttu-id="c9e11-237">Tente outro alias de email em um provedor de email diferente (Microsoft, Yahoo, Gmail, etc.)</span><span class="sxs-lookup"><span data-stu-id="c9e11-237">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="c9e11-238">Criar um [aplicativo de console para enviar email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).</span><span class="sxs-lookup"><span data-stu-id="c9e11-238">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).</span></span>
* <span data-ttu-id="c9e11-239">Tente enviar para contas de email diferente.</span><span class="sxs-lookup"><span data-stu-id="c9e11-239">Try sending to different email accounts.</span></span>

<span data-ttu-id="c9e11-240">**Observação:** uma prática recomendada é não usar segredos de produção no desenvolvimento e teste.</span><span class="sxs-lookup"><span data-stu-id="c9e11-240">**Note:** A security best practice is to not use production secrets in test and development.</span></span> <span data-ttu-id="c9e11-241">Se você publicar o aplicativo no Azure, você pode definir os segredos do SendGrid como configurações de aplicativo no portal do aplicativo Web do Azure.</span><span class="sxs-lookup"><span data-stu-id="c9e11-241">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="c9e11-242">O sistema de configuração está configurado para ler as chaves de variáveis de ambiente.</span><span class="sxs-lookup"><span data-stu-id="c9e11-242">The configuration system is setup to read keys from environment variables.</span></span>

## <a name="prevent-login-at-registration"></a><span data-ttu-id="c9e11-243">Impedir que o logon no momento do registro</span><span class="sxs-lookup"><span data-stu-id="c9e11-243">Prevent login at registration</span></span>

<span data-ttu-id="c9e11-244">Com os modelos atuais, depois que o usuário concluir o formulário de registro, estão conectados (autenticados).</span><span class="sxs-lookup"><span data-stu-id="c9e11-244">With the current templates, once a user completes the registration form, they're logged in (authenticated).</span></span> <span data-ttu-id="c9e11-245">Você geralmente deseja confirmar seu email antes de registrá-los.</span><span class="sxs-lookup"><span data-stu-id="c9e11-245">You generally want to confirm their email before logging them in.</span></span> <span data-ttu-id="c9e11-246">A seção a seguir, vamos modificar o código para exigir que novos usuários possuem um email confirmado antes que ele estão conectados.</span><span class="sxs-lookup"><span data-stu-id="c9e11-246">In the section below, we will modify the code to require new users have a confirmed email before they're logged in.</span></span> <span data-ttu-id="c9e11-247">Atualização de `[HttpPost] Login` ação no *AccountController.cs* arquivo com as seguintes alterações realçados.</span><span class="sxs-lookup"><span data-stu-id="c9e11-247">Update the `[HttpPost] Login` action in the *AccountController.cs* file with the following highlighted changes.</span></span>

[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=11-21&name=snippet_Login)]

<span data-ttu-id="c9e11-248">**Observação:** uma prática recomendada é não usar segredos de produção no desenvolvimento e teste.</span><span class="sxs-lookup"><span data-stu-id="c9e11-248">**Note:** A security best practice is to not use production secrets in test and development.</span></span> <span data-ttu-id="c9e11-249">Se você publicar o aplicativo no Azure, você pode definir os segredos do SendGrid como configurações de aplicativo no portal do aplicativo Web do Azure.</span><span class="sxs-lookup"><span data-stu-id="c9e11-249">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="c9e11-250">O sistema de configuração está configurado para ler as chaves de variáveis de ambiente.</span><span class="sxs-lookup"><span data-stu-id="c9e11-250">The configuration system is setup to read keys from environment variables.</span></span>


## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="c9e11-251">Combinar as contas de logon local e social</span><span class="sxs-lookup"><span data-stu-id="c9e11-251">Combine social and local login accounts</span></span>

<span data-ttu-id="c9e11-252">Observação: Esta seção se aplica apenas ao ASP.NET Core 1. x.</span><span class="sxs-lookup"><span data-stu-id="c9e11-252">Note: This section applies only to ASP.NET Core 1.x.</span></span> <span data-ttu-id="c9e11-253">Para o ASP.NET Core 2. x, consulte [isso](https://github.com/aspnet/Docs/issues/3753) problema.</span><span class="sxs-lookup"><span data-stu-id="c9e11-253">For ASP.NET Core 2.x, see [this](https://github.com/aspnet/Docs/issues/3753) issue.</span></span>

<span data-ttu-id="c9e11-254">Para concluir esta seção, você deve primeiro habilitar um provedor de autenticação externa.</span><span class="sxs-lookup"><span data-stu-id="c9e11-254">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="c9e11-255">Consulte [habilitando a autenticação usando o Facebook, Google e outros provedores externos](social/index.md).</span><span class="sxs-lookup"><span data-stu-id="c9e11-255">See [Enabling authentication using Facebook, Google and other external providers](social/index.md).</span></span>

<span data-ttu-id="c9e11-256">Você pode combinar as contas locais e sociais clicando no link seu email.</span><span class="sxs-lookup"><span data-stu-id="c9e11-256">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="c9e11-257">Na sequência a seguir, "RickAndMSFT@gmail.com" é criado como um logon local; no entanto, você pode criar a conta como um logon social primeiro, depois de adicionar um logon local.</span><span class="sxs-lookup"><span data-stu-id="c9e11-257">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Aplicativo Web: RickAndMSFT@gmail.com usuário autenticado](accconfirm/_static/rick.png)

<span data-ttu-id="c9e11-259">Clique no **gerenciar** link.</span><span class="sxs-lookup"><span data-stu-id="c9e11-259">Click on the **Manage** link.</span></span> <span data-ttu-id="c9e11-260">Observe externo 0 (logons sociais) associada à conta.</span><span class="sxs-lookup"><span data-stu-id="c9e11-260">Note the 0 external (social logins) associated with this account.</span></span>

![Gerenciar o modo de exibição](accconfirm/_static/manage.png)

<span data-ttu-id="c9e11-262">Clique no link para outro serviço de logon e aceitar as solicitações do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="c9e11-262">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="c9e11-263">Na imagem abaixo, o Facebook é o provedor de autenticação externa:</span><span class="sxs-lookup"><span data-stu-id="c9e11-263">In the image below, Facebook is the external authentication provider:</span></span>

![Gerenciar o modo de exibição de logons externos listando Facebook](accconfirm/_static/fb.png)

<span data-ttu-id="c9e11-265">As duas contas foram combinadas.</span><span class="sxs-lookup"><span data-stu-id="c9e11-265">The two accounts have been combined.</span></span> <span data-ttu-id="c9e11-266">Você poderá fazer logon com a conta.</span><span class="sxs-lookup"><span data-stu-id="c9e11-266">You will be able to log on with either account.</span></span> <span data-ttu-id="c9e11-267">Convém que os usuários adicionem contas locais no caso de seu log social no serviço de autenticação está inoperante ou mais provável perdeu o acesso à sua conta social.</span><span class="sxs-lookup"><span data-stu-id="c9e11-267">You might want your users to add local accounts in case their social log in authentication service is down, or more likely they've lost access to their social account.</span></span>
