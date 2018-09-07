---
title: Introdução à identidade do ASP.NET Core
author: rick-anderson
description: Use identidade com um aplicativo ASP.NET Core. Saiba como definir os requisitos de senha (RequireDigit, RequiredLength, RequiredUniqueChars e muito mais).
ms.author: riande
ms.date: 08/08/2018
uid: security/authentication/identity
ms.openlocfilehash: 96f446ad9ec1ef5d807a8648e68308ee20583365
ms.sourcegitcommit: 08bf41d4b3e696ab512b044970e8304816f8cc56
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/06/2018
ms.locfileid: "44040022"
---
# <a name="introduction-to-identity-on-aspnet-core"></a><span data-ttu-id="5a708-104">Introdução à identidade do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5a708-104">Introduction to Identity on ASP.NET Core</span></span>

<span data-ttu-id="5a708-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="5a708-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="5a708-106">O ASP.NET Core Identity é um sistema de associação que adiciona a funcionalidade de logon para aplicativos ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5a708-106">ASP.NET Core Identity is a membership system that adds login functionality to ASP.NET Core apps.</span></span> <span data-ttu-id="5a708-107">Os usuários podem criar uma conta com as informações de logon armazenadas no Identity ou eles podem usar um provedor de logon externo.</span><span class="sxs-lookup"><span data-stu-id="5a708-107">Users can create an account with the login information stored in Identity or they can use an external login provider.</span></span> <span data-ttu-id="5a708-108">Provedores de logon externo com suporte incluem [Facebook, Google, Account da Microsoft e Twitter](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="5a708-108">Supported external login providers include [Facebook, Google, Microsoft Account, and Twitter](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="5a708-109">Identidade pode ser configurada usando um banco de dados do SQL Server para armazenar nomes de usuário, senhas e dados de perfil.</span><span class="sxs-lookup"><span data-stu-id="5a708-109">Identity can be configured using a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="5a708-110">Como alternativa, outro repositório persistente pode ser usado, por exemplo, o armazenamento de tabelas do Azure.</span><span class="sxs-lookup"><span data-stu-id="5a708-110">Alternatively, another persistent store can be used, for example, Azure Table Storage.</span></span>

[<span data-ttu-id="5a708-111">Exibir ou baixar o código de exemplo.</span><span class="sxs-lookup"><span data-stu-id="5a708-111">View or download the sample code.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) [<span data-ttu-id="5a708-112">(Como fazer o download)</span><span class="sxs-lookup"><span data-stu-id="5a708-112">(How to download)</span></span>](xref:tutorials/index#how-to-download-a-sample)

<span data-ttu-id="5a708-113">Neste tópico, saiba como usar a identidade para registrar, faça logon e logoff de um usuário.</span><span class="sxs-lookup"><span data-stu-id="5a708-113">In this topic, you learn how to use Identity to register, log in, and log out a user.</span></span> <span data-ttu-id="5a708-114">Para obter instruções mais detalhadas sobre como criar aplicativos que usam a identidade, consulte a seção próximas etapas no final deste artigo.</span><span class="sxs-lookup"><span data-stu-id="5a708-114">For more detailed instructions about creating apps that use Identity, see the Next Steps section at the end of this article.</span></span>

## <a name="create-a-web-app-with-authentication"></a><span data-ttu-id="5a708-115">Criar um aplicativo Web com autenticação</span><span class="sxs-lookup"><span data-stu-id="5a708-115">Create a Web app with authentication</span></span>

<span data-ttu-id="5a708-116">Crie um projeto de aplicativo Web ASP.NET Core com contas de usuário individuais.</span><span class="sxs-lookup"><span data-stu-id="5a708-116">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5a708-117">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5a708-117">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="5a708-118">Selecione **Arquivo** > **Novo** > **Projeto**.</span><span class="sxs-lookup"><span data-stu-id="5a708-118">Select **File** > **New** > **Project**.</span></span> 
* <span data-ttu-id="5a708-119">Selecione **Aplicativo Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="5a708-119">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="5a708-120">Nomeie o projeto **WebApp1** para ter o mesmo namespace que o download do projeto.</span><span class="sxs-lookup"><span data-stu-id="5a708-120">Name the project **WebApp1** to have the same namespace as the project download.</span></span> <span data-ttu-id="5a708-121">Clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="5a708-121">Click **OK**.</span></span>
* <span data-ttu-id="5a708-122">Selecione um ASP.NET Core **aplicativo Web** para o ASP.NET Core 2.1, em seguida, selecione **alterar autenticação**.</span><span class="sxs-lookup"><span data-stu-id="5a708-122">Select an ASP.NET Core **Web Application** for ASP.NET Core 2.1, then select **Change Authentication**.</span></span>
* <span data-ttu-id="5a708-123">Selecione **contas de usuário individuais** e clique em **Okey**.</span><span class="sxs-lookup"><span data-stu-id="5a708-123">Select **Individual User Accounts** and click **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="5a708-124">CLI do .NET Core</span><span class="sxs-lookup"><span data-stu-id="5a708-124">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet new webapp --auth Individual -o WebApp1
```

---

<span data-ttu-id="5a708-125">Fornece o projeto gerado [ASP.NET Core Identity](xref:security/authentication/identity) como um [biblioteca de classes Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="5a708-125">The generated project provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span>

### <a name="test-register-and-login"></a><span data-ttu-id="5a708-126">Registro de teste e de logon</span><span class="sxs-lookup"><span data-stu-id="5a708-126">Test Register and Login</span></span>

<span data-ttu-id="5a708-127">Execute o aplicativo e registrar um usuário.</span><span class="sxs-lookup"><span data-stu-id="5a708-127">Run the app and register a user.</span></span> <span data-ttu-id="5a708-128">Dependendo do tamanho da tela, você talvez precise selecionar o botão de alternância de navegação para ver os **registre** e **Login** links.</span><span class="sxs-lookup"><span data-stu-id="5a708-128">Depending on your screen size, you might need to select the navigation toggle button to see the **Register** and **Login** links.</span></span>

![botão de barra de navegação de alternância](identity/_static/navToggle.png)

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>
### <a name="configure-identity-services"></a><span data-ttu-id="5a708-130">Configurar os serviços de identidade</span><span class="sxs-lookup"><span data-stu-id="5a708-130">Configure Identity services</span></span>

<span data-ttu-id="5a708-131">Serviços são adicionados no `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="5a708-131">Services are added in `ConfigureServices`.</span></span> <span data-ttu-id="5a708-132">O padrão típico é chamar todos os `Add{Service}` métodos e, em seguida, chame o `services.Configure{Service}` métodos.</span><span class="sxs-lookup"><span data-stu-id="5a708-132">The typical pattern is to call all the `Add{Service}` methods, and then call all the `services.Configure{Service}` methods.</span></span> <span data-ttu-id="5a708-133">O código a seguir não inclui o modelo gerado `CookiePolicyOptions`:</span><span class="sxs-lookup"><span data-stu-id="5a708-133">The following code doesn't include the template generated `CookiePolicyOptions`:</span></span>

::: moniker range=">= aspnetcore-2.1"

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Startup.cs?name=snippet_configureservices)]

<span data-ttu-id="5a708-134">O código anterior configura a identidade com os valores de opção padrão.</span><span class="sxs-lookup"><span data-stu-id="5a708-134">The preceding code configures Identity with default option values.</span></span> <span data-ttu-id="5a708-135">Serviços são disponibilizados para o aplicativo por meio [injeção de dependência](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="5a708-135">Services are made available to the app through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="5a708-136">Identidade é habilitada por meio da chamada [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span><span class="sxs-lookup"><span data-stu-id="5a708-136">Identity is enabled by calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span> <span data-ttu-id="5a708-137">`UseAuthentication` Adiciona a autenticação [middleware](xref:fundamentals/middleware/index) ao pipeline de solicitação.</span><span class="sxs-lookup"><span data-stu-id="5a708-137">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Startup.cs?name=snippet_configure&highlight=18)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   <span data-ttu-id="5a708-138">Serviços são disponibilizados para o aplicativo por meio [injeção de dependência](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="5a708-138">Services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="5a708-139">Identidade é habilitada para o aplicativo, chamando `UseAuthentication` no `Configure` método.</span><span class="sxs-lookup"><span data-stu-id="5a708-139">Identity is enabled for the application by calling `UseAuthentication` in the `Configure` method.</span></span> <span data-ttu-id="5a708-140">`UseAuthentication` Adiciona a autenticação [middleware](xref:fundamentals/middleware/index) ao pipeline de solicitação.</span><span class="sxs-lookup"><span data-stu-id="5a708-140">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-33)]

   <span data-ttu-id="5a708-141">Esses serviços são disponibilizados para o aplicativo por meio [injeção de dependência](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="5a708-141">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="5a708-142">Identidade é habilitada para o aplicativo, chamando `UseIdentity` no `Configure` método.</span><span class="sxs-lookup"><span data-stu-id="5a708-142">Identity is enabled for the application by calling `UseIdentity` in the `Configure` method.</span></span> <span data-ttu-id="5a708-143">`UseIdentity` Adiciona a autenticação baseada em cookie [middleware](xref:fundamentals/middleware/index) ao pipeline de solicitação.</span><span class="sxs-lookup"><span data-stu-id="5a708-143">`UseIdentity` adds cookie-based authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]

::: moniker-end

<span data-ttu-id="5a708-144">Para obter mais informações, consulte o [classe IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) e [inicialização do aplicativo](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="5a708-144">For more information, see the [IdentityOptions Class](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) and [Application Startup](xref:fundamentals/startup).</span></span>

## <a name="scaffold-register-login-and-logout"></a><span data-ttu-id="5a708-145">Registre-se de Scaffold, logon e logoff</span><span class="sxs-lookup"><span data-stu-id="5a708-145">Scaffold Register, Login, and LogOut</span></span>

<span data-ttu-id="5a708-146">Siga as [criar o scaffolding de identidade em um projeto do Razor com autorização](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) instruções.</span><span class="sxs-lookup"><span data-stu-id="5a708-146">Follow the [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) instructions.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5a708-147">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5a708-147">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="5a708-148">Adicione os arquivos de registro, logon e logoff.</span><span class="sxs-lookup"><span data-stu-id="5a708-148">Add the Register, Login, and LogOut files.</span></span>


# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="5a708-149">CLI do .NET Core</span><span class="sxs-lookup"><span data-stu-id="5a708-149">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="5a708-150">Se você criou o projeto com o nome **WebApp1**, execute os seguintes comandos.</span><span class="sxs-lookup"><span data-stu-id="5a708-150">If you created the project with name **WebApp1**, run the following commands.</span></span> <span data-ttu-id="5a708-151">Caso contrário, use o namespace correto para o `ApplicationDbContext`:</span><span class="sxs-lookup"><span data-stu-id="5a708-151">Otherwise, use the correct namespace for the `ApplicationDbContext`:</span></span>


```cli
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"

```

<span data-ttu-id="5a708-152">O PowerShell usa o ponto e vírgula como separador de comando.</span><span class="sxs-lookup"><span data-stu-id="5a708-152">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="5a708-153">Ao usar o PowerShell, a ponto e vírgula na lista de arquivos de escape ou coloque a lista de arquivos entre aspas duplas, como mostra o exemplo anterior.</span><span class="sxs-lookup"><span data-stu-id="5a708-153">When using PowerShell, escape the semicolons in the file list or put the file list in double quotes, as the preceding example shows.</span></span>

---

### <a name="examine-register"></a><span data-ttu-id="5a708-154">Registre-se de examinar</span><span class="sxs-lookup"><span data-stu-id="5a708-154">Examine Register</span></span>

::: moniker range=">= aspnetcore-2.1"

   <span data-ttu-id="5a708-155">Quando um usuário clica o **registre** link, o `RegisterModel.OnPostAsync` ação é invocada.</span><span class="sxs-lookup"><span data-stu-id="5a708-155">When a user clicks the **Register** link, the `RegisterModel.OnPostAsync` action is invoked.</span></span> <span data-ttu-id="5a708-156">O usuário é criado pelo [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) sobre o `_userManager` objeto.</span><span class="sxs-lookup"><span data-stu-id="5a708-156">The user is created by [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) on the `_userManager` object.</span></span> <span data-ttu-id="5a708-157">`_userManager` é fornecido pela injeção de dependência):</span><span class="sxs-lookup"><span data-stu-id="5a708-157">`_userManager` is provided by dependency injection):</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Register.cshtml.cs?name=snippet&highlight=7,22)]

::: moniker-end
::: moniker range="= aspnetcore-2.0"

   <span data-ttu-id="5a708-158">Quando um usuário clica o **registre** link, o `Register` ação é invocada em `AccountController`.</span><span class="sxs-lookup"><span data-stu-id="5a708-158">When a user clicks the **Register** link, the `Register` action is invoked on `AccountController`.</span></span> <span data-ttu-id="5a708-159">O `Register` ação cria o usuário chamando `CreateAsync` sobre o `_userManager` objeto (fornecidas para `AccountController` pela injeção de dependência):</span><span class="sxs-lookup"><span data-stu-id="5a708-159">The `Register` action creates the user by calling `CreateAsync` on the `_userManager` object (provided to `AccountController` by dependency injection):</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

::: moniker-end

   <span data-ttu-id="5a708-160">Se o usuário foi criado com êxito, o usuário é conectado pela chamada para `_signInManager.SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="5a708-160">If the user was created successfully, the user is logged in by the call to `_signInManager.SignInAsync`.</span></span>

   <span data-ttu-id="5a708-161">**Observação:** consulte a [confirmação de conta](xref:security/authentication/accconfirm#prevent-login-at-registration) para verificar as etapas para impedir o logon imediato no registro.</span><span class="sxs-lookup"><span data-stu-id="5a708-161">**Note:** See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>

### <a name="log-in"></a><span data-ttu-id="5a708-162">Fazer Logon</span><span class="sxs-lookup"><span data-stu-id="5a708-162">Log in</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="5a708-163">O formulário de logon é exibido quando:</span><span class="sxs-lookup"><span data-stu-id="5a708-163">The Login form is displayed when:</span></span>

* <span data-ttu-id="5a708-164">O **faça logon no** link é selecionado.</span><span class="sxs-lookup"><span data-stu-id="5a708-164">The **Log in** link  is selected.</span></span>
* <span data-ttu-id="5a708-165">Quando um usuário acessa uma página em que eles não são autenticados **ou** autorizado, eles são redirecionados para a página de logon.</span><span class="sxs-lookup"><span data-stu-id="5a708-165">When a user accesses a page where they are not authenticated **or** authorized, they are redirected to the Login page.</span></span> 

<span data-ttu-id="5a708-166">Quando o formulário na página de logon é enviado, o `OnPostAsync` ação é chamada.</span><span class="sxs-lookup"><span data-stu-id="5a708-166">When the form on the Login page is submitted, the `OnPostAsync` action is called.</span></span> <span data-ttu-id="5a708-167">`PasswordSignInAsync` é chamado de `_signInManager` (fornecido pela injeção de dependência) do objeto.</span><span class="sxs-lookup"><span data-stu-id="5a708-167">`PasswordSignInAsync` is called on the `_signInManager` object (provided by dependency injection).</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Login.cshtml.cs?name=snippet&highlight=10-11)]

   <span data-ttu-id="5a708-168">A base `Controller` classe expõe um `User` propriedade que você pode acessar métodos do controlador.</span><span class="sxs-lookup"><span data-stu-id="5a708-168">The base `Controller` class exposes a `User` property that you can access from controller methods.</span></span> <span data-ttu-id="5a708-169">Por exemplo, você pode enumerar `User.Claims` e tomar decisões de autorização.</span><span class="sxs-lookup"><span data-stu-id="5a708-169">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="5a708-170">Para obter mais informações, consulte [autorização](xref:security/authorization/index).</span><span class="sxs-lookup"><span data-stu-id="5a708-170">For more information, see [Authorization](xref:security/authorization/index).</span></span>

::: moniker-end
::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="5a708-171">O formulário de logon é exibido quando os usuários selecionam o **faça logon no** vincular ou será redirecionado ao acessar uma página que requer autenticação.</span><span class="sxs-lookup"><span data-stu-id="5a708-171">The Login form is displayed when users select the **Log in** link or are redirected when accessing a page that requires authentication.</span></span> <span data-ttu-id="5a708-172">Quando o usuário envia o formulário na página de login, a ação`AccountController` `Login` é chamada.</span><span class="sxs-lookup"><span data-stu-id="5a708-172">When the user submits the form on the Login page, the `AccountController` `Login` action is called.</span></span>

<span data-ttu-id="5a708-173">O `Login` faz chamadas `PasswordSignInAsync` no objeto `_signInManager` (fornecido pelo `AccountController` por injeção de dependência).</span><span class="sxs-lookup"><span data-stu-id="5a708-173">The `Login` action calls `PasswordSignInAsync` on the `_signInManager` object (provided to `AccountController` by dependency injection).</span></span>

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]

<span data-ttu-id="5a708-174">A base de (`Controller` ou `PageModel`) classe expõe um `User` propriedade.</span><span class="sxs-lookup"><span data-stu-id="5a708-174">The base (`Controller` or `PageModel`) class exposes a `User` property.</span></span> <span data-ttu-id="5a708-175">Por exemplo, `User.Claims` podem ser enumeradas para tomar decisões de autorização.</span><span class="sxs-lookup"><span data-stu-id="5a708-175">For example, `User.Claims` can be enumerated to make authorization decisions.</span></span>

::: moniker-end

### <a name="log-out"></a><span data-ttu-id="5a708-176">Fazer logoff</span><span class="sxs-lookup"><span data-stu-id="5a708-176">Log out</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="5a708-177">O **fazer logoff** link invoca o `LogoutModel.OnPost` ação.</span><span class="sxs-lookup"><span data-stu-id="5a708-177">The **Log out** link invokes the `LogoutModel.OnPost` action.</span></span> 

[!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Logout.cshtml.cs)]

<span data-ttu-id="5a708-178">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) limpa as declarações do usuário armazenadas em um cookie.</span><span class="sxs-lookup"><span data-stu-id="5a708-178">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) clears the user's claims stored in a cookie.</span></span> <span data-ttu-id="5a708-179">Não redirecionar após chamar `SignOutAsync` ou o usuário irá **não** ser desconectado.</span><span class="sxs-lookup"><span data-stu-id="5a708-179">Don't redirect after calling `SignOutAsync` or the user will **not** be signed out.</span></span>

<span data-ttu-id="5a708-180">POST é especificado na *Pages/Shared/_LoginPartial.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="5a708-180">Post is specified in the *Pages/Shared/_LoginPartial.cshtml*:</span></span>

[!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/_LoginPartial.cshtml?highlight=10)]

::: moniker-end
::: moniker range="= aspnetcore-2.0"
   <span data-ttu-id="5a708-181">Clicar a **fazer logoff** vincular chamadas a `LogOut` ação.</span><span class="sxs-lookup"><span data-stu-id="5a708-181">Clicking the **Log out** link calls the `LogOut` action.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]

   <span data-ttu-id="5a708-182">O código anterior chama o `_signInManager.SignOutAsync` método.</span><span class="sxs-lookup"><span data-stu-id="5a708-182">The preceding code calls the `_signInManager.SignOutAsync` method.</span></span> <span data-ttu-id="5a708-183">O `SignOutAsync` método limpa as declarações do usuário armazenadas em um cookie.</span><span class="sxs-lookup"><span data-stu-id="5a708-183">The `SignOutAsync` method clears the user's claims stored in a cookie.</span></span>
::: moniker-end

## <a name="test-identity"></a><span data-ttu-id="5a708-184">Identidade de teste</span><span class="sxs-lookup"><span data-stu-id="5a708-184">Test Identity</span></span>

<span data-ttu-id="5a708-185">Os modelos de projeto da web padrão permitem acesso anônimo para as home pages.</span><span class="sxs-lookup"><span data-stu-id="5a708-185">The default web project templates allow anonymous access to the home pages.</span></span> <span data-ttu-id="5a708-186">Para testar a identidade, adicione [ `[Authorize]` ](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) para a página sobre.</span><span class="sxs-lookup"><span data-stu-id="5a708-186">To test Identity, add [`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) to the About page.</span></span>

[!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/About.cshtml.cs)]

<span data-ttu-id="5a708-187">Se você estiver entrado, saia do serviço. Execute o aplicativo e selecione o **sobre** link.</span><span class="sxs-lookup"><span data-stu-id="5a708-187">If you are signed in, sign out. Run the app and select the **About** link.</span></span> <span data-ttu-id="5a708-188">Você será redirecionado à página de logon.</span><span class="sxs-lookup"><span data-stu-id="5a708-188">You are redirected to the login page.</span></span>

::: moniker range=">= aspnetcore-2.1"

### <a name="explore-identity"></a><span data-ttu-id="5a708-189">Explore a identidade</span><span class="sxs-lookup"><span data-stu-id="5a708-189">Explore Identity</span></span>

<span data-ttu-id="5a708-190">Para explorar a identidade em mais detalhes:</span><span class="sxs-lookup"><span data-stu-id="5a708-190">To explore Identity in more detail:</span></span>

* [<span data-ttu-id="5a708-191">Criar fonte de interface do usuário de identidade completa</span><span class="sxs-lookup"><span data-stu-id="5a708-191">Create full identity UI source</span></span>](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* <span data-ttu-id="5a708-192">Examine a origem de cada página e percorrer o depurador.</span><span class="sxs-lookup"><span data-stu-id="5a708-192">Examine the source of each page and step through the debugger.</span></span>

::: moniker-end

## <a name="identity-components"></a><span data-ttu-id="5a708-193">Componentes de identidade</span><span class="sxs-lookup"><span data-stu-id="5a708-193">Identity Components</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="5a708-194">Todos os identidade NuGet pacotes dependentes são incluídos na [metapacote Microsoft](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="5a708-194">All the Identity dependent NuGet packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
::: moniker-end

<span data-ttu-id="5a708-195">O pacote principal para a identidade está [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span><span class="sxs-lookup"><span data-stu-id="5a708-195">The primary package for Identity is [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span></span> <span data-ttu-id="5a708-196">Este pacote contém o conjunto principal de interfaces para ASP.NET Core Identity e é incluído por `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="5a708-196">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="5a708-197">Migrando para o ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="5a708-197">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="5a708-198">Para obter mais informações e diretrizes sobre como migrar o armazenamento de identidade existente, consulte [migrar autenticação e identidade](xref:migration/identity).</span><span class="sxs-lookup"><span data-stu-id="5a708-198">For more information and guidance on migrating your existing Identity store, see [Migrate Authentication and Identity](xref:migration/identity).</span></span>

## <a name="setting-password-strength"></a><span data-ttu-id="5a708-199">Definindo a força da senha</span><span class="sxs-lookup"><span data-stu-id="5a708-199">Setting password strength</span></span>

<span data-ttu-id="5a708-200">Consulte [configuração](#pw) para obter um exemplo que defina os requisitos mínimos de senha.</span><span class="sxs-lookup"><span data-stu-id="5a708-200">See [Configuration](#pw) for a sample that sets the minimum password requirements.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5a708-201">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="5a708-201">Next Steps</span></span>

* [<span data-ttu-id="5a708-202">Configurar o Identity</span><span class="sxs-lookup"><span data-stu-id="5a708-202">Configure Identity</span></span>](xref:security/authentication/identity-configuration)
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* <span data-ttu-id="5a708-203">[Configurar o tipo de dados de chaves primárias de identidade](xref:security/authentication/identity-primary-key-configuration).</span><span class="sxs-lookup"><span data-stu-id="5a708-203">[Configure Identity primary keys data type](xref:security/authentication/identity-primary-key-configuration).</span></span>
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>
