---
title: "Introdução à identidade no núcleo do ASP.NET"
author: rick-anderson
description: Usar identidade com um aplicativo do ASP.NET Core
keywords: "Autorização de ASP.NET Core, identidade, segurança"
ms.author: riande
manager: wpickett
ms.date: 01/02/2018
ms.topic: article
ms.assetid: cf119f21-1a2b-49a2-b052-547ccb66ee83
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity
ms.openlocfilehash: 7af53bfad2b77558a06003cbc6534236235054c4
ms.sourcegitcommit: 677986b3a39817b712e2432cce85ad1685326b75
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/04/2018
---
# <a name="introduction-to-identity-on-aspnet-core"></a><span data-ttu-id="15bdc-104">Introdução à identidade no núcleo do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="15bdc-104">Introduction to Identity on ASP.NET Core</span></span>

<span data-ttu-id="15bdc-105">Por [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), Jon Galloway [Erik Reitan](https://github.com/Erikre), e [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="15bdc-105">By [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), Jon Galloway, [Erik Reitan](https://github.com/Erikre), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="15bdc-106">Identidade do ASP.NET Core é um sistema de associação que permite que você adicionar a funcionalidade de logon ao seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="15bdc-106">ASP.NET Core Identity is a membership system which allows you to add login functionality to your application.</span></span> <span data-ttu-id="15bdc-107">Os usuários podem criar uma conta e logon com um nome de usuário e senha ou eles podem usar um provedor de logon externo, como Facebook, Google, Microsoft Account, Twitter ou outras pessoas.</span><span class="sxs-lookup"><span data-stu-id="15bdc-107">Users can create an account and login with a user name and password or they can use an external login provider such as Facebook, Google, Microsoft Account, Twitter or others.</span></span>

<span data-ttu-id="15bdc-108">Você pode configurar a identidade do ASP.NET Core para usar um banco de dados do SQL Server para armazenar os nomes de usuário, senhas e dados de perfil.</span><span class="sxs-lookup"><span data-stu-id="15bdc-108">You can configure ASP.NET Core Identity to use a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="15bdc-109">Como alternativa, você pode usar seu próprio armazenamento persistente, por exemplo, um armazenamento de tabela do Azure.</span><span class="sxs-lookup"><span data-stu-id="15bdc-109">Alternatively, you can use your own persistent store, for example, an Azure Table Storage.</span></span> <span data-ttu-id="15bdc-110">Este documento contém instruções para Visual Studio e usando a CLI.</span><span class="sxs-lookup"><span data-stu-id="15bdc-110">This document contains instructions for Visual Studio and for using the CLI.</span></span>

## <a name="overview-of-identity"></a><span data-ttu-id="15bdc-111">Visão geral da identidade</span><span class="sxs-lookup"><span data-stu-id="15bdc-111">Overview of Identity</span></span>

<span data-ttu-id="15bdc-112">Neste tópico, você vai aprender a usar a identidade do ASP.NET Core para adicionar funcionalidade ao se registrar, faça logon no e fazer logoff de um usuário.</span><span class="sxs-lookup"><span data-stu-id="15bdc-112">In this topic, you'll learn how to use ASP.NET Core Identity to add functionality to register, log in, and log out a user.</span></span> <span data-ttu-id="15bdc-113">Para obter instruções mais detalhadas sobre a criação de aplicativos usando a identidade do ASP.NET Core, consulte a seção próximas etapas no final deste artigo.</span><span class="sxs-lookup"><span data-stu-id="15bdc-113">For more detailed instructions about creating apps using ASP.NET Core Identity, see the Next Steps section at the end of this article.</span></span>

1.  <span data-ttu-id="15bdc-114">Crie um projeto de aplicativo Web do ASP.NET Core com contas de usuário individuais.</span><span class="sxs-lookup"><span data-stu-id="15bdc-114">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

    # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="15bdc-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="15bdc-115">Visual Studio</span></span>](#tab/visual-studio)
    <span data-ttu-id="15bdc-116">No Visual Studio, selecione **arquivo** -> **novo** -> **projeto**.</span><span class="sxs-lookup"><span data-stu-id="15bdc-116">In Visual Studio, select **File** -> **New** -> **Project**.</span></span> <span data-ttu-id="15bdc-117">Selecione **aplicativo Web do ASP.NET Core** e clique em **Okey**.</span><span class="sxs-lookup"><span data-stu-id="15bdc-117">Select **ASP.NET Core Web Application** and click **OK**.</span></span> 

    ![Caixa de diálogo Novo Projeto](identity/_static/01-new-project.png)

    <span data-ttu-id="15bdc-119">Selecione um ASP.NET Core **aplicativo Web (Model-View-Controller)** do ASP.NET Core 2. x, em seguida, selecione **alterar autenticação**.</span><span class="sxs-lookup"><span data-stu-id="15bdc-119">Select an ASP.NET Core **Web Application (Model-View-Controller)** for ASP.NET Core 2.x, then select **Change Authentication**.</span></span> 

    ![Caixa de diálogo Novo Projeto](identity/_static/02-new-project.png)

    <span data-ttu-id="15bdc-121">Oferta de será exibida uma caixa de diálogo Opções de autenticação.</span><span class="sxs-lookup"><span data-stu-id="15bdc-121">A dialog appears offering authentication choices.</span></span> <span data-ttu-id="15bdc-122">Selecione **contas de usuário individuais** e clique em **Okey** para retornar à caixa de diálogo anterior.</span><span class="sxs-lookup"><span data-stu-id="15bdc-122">Select **Individual User Accounts** and click **OK** to return to the previous dialog.</span></span>

    ![Caixa de diálogo Novo Projeto](identity/_static/03-new-project-auth.png)
    
    <span data-ttu-id="15bdc-124">Selecionando **contas de usuário individuais** direciona o Visual Studio para criar modelos, ViewModels, modos de exibição, controladores e outros ativos necessários para a autenticação como parte do modelo de projeto.</span><span class="sxs-lookup"><span data-stu-id="15bdc-124">Selecting **Individual User Accounts** directs Visual Studio to create Models, ViewModels, Views, Controllers, and other assets required for authentication as part of the project template.</span></span>
 
    
    # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="15bdc-125">CLI do .NET Core</span><span class="sxs-lookup"><span data-stu-id="15bdc-125">.NET Core CLI</span></span>](#tab/netcore-cli)
    <span data-ttu-id="15bdc-126">Se usar o .NET Core CLI, crie o novo projeto usando ``dotnet new mvc --auth Individual``.</span><span class="sxs-lookup"><span data-stu-id="15bdc-126">If using the .NET Core CLI, create the new project using ``dotnet new mvc --auth Individual``.</span></span> <span data-ttu-id="15bdc-127">Este comando cria um novo projeto com o mesmo código de modelo de identidade que Visual Studio cria.</span><span class="sxs-lookup"><span data-stu-id="15bdc-127">This command creates a new project with the same Identity template code Visual Studio creates.</span></span>
 
    <span data-ttu-id="15bdc-128">O projeto criado contém o `Microsoft.AspNetCore.Identity.EntityFrameworkCore` pacote, persiste os dados de identidade e o esquema para o SQL Server usando [Entity Framework Core](https://docs.microsoft.com/ef/).</span><span class="sxs-lookup"><span data-stu-id="15bdc-128">The created project contains the `Microsoft.AspNetCore.Identity.EntityFrameworkCore` package, which persists the Identity data and schema to SQL Server using [Entity Framework Core](https://docs.microsoft.com/ef/).</span></span>
    
    ---
 
2.  <span data-ttu-id="15bdc-129">Configurar os serviços de identidade e adicionar middleware em `Startup`.</span><span class="sxs-lookup"><span data-stu-id="15bdc-129">Configure Identity services and add middleware in `Startup`.</span></span>

    <span data-ttu-id="15bdc-130">Os serviços de identidade são adicionados ao aplicativo o `ConfigureServices` método o `Startup` classe:</span><span class="sxs-lookup"><span data-stu-id="15bdc-130">The Identity services are added to the application in the `ConfigureServices` method in the `Startup` class:</span></span>

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="15bdc-131">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="15bdc-131">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-39)]
    
    <span data-ttu-id="15bdc-132">Esses serviços são disponibilizados para o aplicativo por meio de [injeção de dependência](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="15bdc-132">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>
    
    <span data-ttu-id="15bdc-133">Identidade é habilitada para o aplicativo chamando `UseAuthentication` no `Configure` método.</span><span class="sxs-lookup"><span data-stu-id="15bdc-133">Identity is enabled for the application by calling `UseAuthentication` in the `Configure` method.</span></span> <span data-ttu-id="15bdc-134">`UseAuthentication`Adiciona autenticação [middleware](xref:fundamentals/middleware) para o pipeline de solicitação.</span><span class="sxs-lookup"><span data-stu-id="15bdc-134">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware) to the request pipeline.</span></span>
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="15bdc-135">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="15bdc-135">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-34)]
    
    <span data-ttu-id="15bdc-136">Esses serviços são disponibilizados para o aplicativo por meio de [injeção de dependência](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="15bdc-136">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>
    
    <span data-ttu-id="15bdc-137">Identidade é habilitada para o aplicativo chamando `UseIdentity` no `Configure` método.</span><span class="sxs-lookup"><span data-stu-id="15bdc-137">Identity is enabled for the application by calling `UseIdentity` in the `Configure` method.</span></span> <span data-ttu-id="15bdc-138">`UseIdentity`Adiciona a autenticação baseada em cookie [middleware](xref:fundamentals/middleware) para o pipeline de solicitação.</span><span class="sxs-lookup"><span data-stu-id="15bdc-138">`UseIdentity` adds cookie-based authentication [middleware](xref:fundamentals/middleware) to the request pipeline.</span></span>
        
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]
    
    ---
     
    <span data-ttu-id="15bdc-139">Para obter mais informações sobre o processo de inicialização do aplicativo, consulte [inicialização do aplicativo](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="15bdc-139">For more information about the application start up process, see [Application Startup](xref:fundamentals/startup).</span></span>

3.  <span data-ttu-id="15bdc-140">Crie um usuário.</span><span class="sxs-lookup"><span data-stu-id="15bdc-140">Create a user.</span></span>

    <span data-ttu-id="15bdc-141">Iniciar o aplicativo e, em seguida, clique no **registrar** link.</span><span class="sxs-lookup"><span data-stu-id="15bdc-141">Launch the application and then click on the **Register** link.</span></span>

    <span data-ttu-id="15bdc-142">Se esta for a primeira vez em que você estiver executando essa ação, talvez seja necessário para executar migrações.</span><span class="sxs-lookup"><span data-stu-id="15bdc-142">If this is the first time you're performing this action, you may be required to run migrations.</span></span> <span data-ttu-id="15bdc-143">O aplicativo solicita que você **migrações aplicar**.</span><span class="sxs-lookup"><span data-stu-id="15bdc-143">The application prompts you to **Apply Migrations**.</span></span> <span data-ttu-id="15bdc-144">Se necessário, atualize a página.</span><span class="sxs-lookup"><span data-stu-id="15bdc-144">Refresh the page if needed.</span></span>
    
    ![Aplicar página da Web de migrações](identity/_static/apply-migrations.png)
    
    <span data-ttu-id="15bdc-146">Como alternativa, você pode testar usando a identidade do ASP.NET Core com seu aplicativo sem um banco de dados persistente usando um banco de dados na memória.</span><span class="sxs-lookup"><span data-stu-id="15bdc-146">Alternately, you can test using ASP.NET Core Identity with your app without a persistent database by using an in-memory database.</span></span> <span data-ttu-id="15bdc-147">Para usar um banco de dados na memória, adicione o ``Microsoft.EntityFrameworkCore.InMemory`` do pacote para seu aplicativo e modifique a chamada do aplicativo para ``AddDbContext`` na ``ConfigureServices`` da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="15bdc-147">To use an in-memory database, add the ``Microsoft.EntityFrameworkCore.InMemory`` package to your app and modify your app's call to ``AddDbContext`` in ``ConfigureServices`` as follows:</span></span>

    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseInMemoryDatabase(Guid.NewGuid().ToString()));
    ```
    
    <span data-ttu-id="15bdc-148">Quando o usuário clica o **registrar** link, o ``Register`` ação é invocada no ``AccountController``.</span><span class="sxs-lookup"><span data-stu-id="15bdc-148">When the user clicks the **Register** link, the ``Register`` action is invoked on ``AccountController``.</span></span> <span data-ttu-id="15bdc-149">O ``Register`` ação cria o usuário chamando `CreateAsync` no `_userManager` objeto (fornecido para ``AccountController`` por injeção de dependência):</span><span class="sxs-lookup"><span data-stu-id="15bdc-149">The ``Register`` action creates the user by calling `CreateAsync` on the  `_userManager` object (provided to ``AccountController`` by dependency injection):</span></span>
 
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

    <span data-ttu-id="15bdc-150">Se o usuário foi criado com êxito, o usuário é conectado pela chamada para ``_signInManager.SignInAsync``.</span><span class="sxs-lookup"><span data-stu-id="15bdc-150">If the user was created successfully, the user is logged in by the call to ``_signInManager.SignInAsync``.</span></span>

    <span data-ttu-id="15bdc-151">**Observação:** consulte [conta confirmação](xref:security/authentication/accconfirm#prevent-login-at-registration) para obter as etapas impedir que o logon imediata no registro.</span><span class="sxs-lookup"><span data-stu-id="15bdc-151">**Note:** See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>
 
4.  <span data-ttu-id="15bdc-152">Iniciar sessão.</span><span class="sxs-lookup"><span data-stu-id="15bdc-152">Log in.</span></span>
 
    <span data-ttu-id="15bdc-153">Os usuários podem entrar clicando o **login** link na parte superior do site, ou pode ser navegados a página de logon se tentarem acessar uma parte do site que requer autorização.</span><span class="sxs-lookup"><span data-stu-id="15bdc-153">Users can sign in by clicking the **Log in** link at the top of the site, or they may be navigated to the Login page if they attempt to access a part of the site that requires authorization.</span></span> <span data-ttu-id="15bdc-154">Quando o usuário envia o formulário na página de logon, o ``AccountController`` ``Login`` ação é chamada.</span><span class="sxs-lookup"><span data-stu-id="15bdc-154">When the user submits the form on the Login page, the ``AccountController`` ``Login`` action is called.</span></span>

    <span data-ttu-id="15bdc-155">O ``Login`` ação chamadas ``PasswordSignInAsync`` no ``_signInManager`` objeto (fornecido para ``AccountController`` por injeção de dependência).</span><span class="sxs-lookup"><span data-stu-id="15bdc-155">The ``Login`` action calls ``PasswordSignInAsync`` on the ``_signInManager`` object (provided to ``AccountController`` by dependency injection).</span></span>

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]
 
    <span data-ttu-id="15bdc-156">A base de ``Controller`` classe expõe um ``User`` propriedade que você pode acessar de métodos do controlador.</span><span class="sxs-lookup"><span data-stu-id="15bdc-156">The base ``Controller`` class exposes a ``User`` property that you can access from controller methods.</span></span> <span data-ttu-id="15bdc-157">Por exemplo, você pode enumerar `User.Claims` e tomar decisões de autorização.</span><span class="sxs-lookup"><span data-stu-id="15bdc-157">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="15bdc-158">Para obter mais informações, consulte [autorização](xref:security/authorization/index).</span><span class="sxs-lookup"><span data-stu-id="15bdc-158">For more information, see [Authorization](xref:security/authorization/index).</span></span>
 
5.  <span data-ttu-id="15bdc-159">Faça logoff.</span><span class="sxs-lookup"><span data-stu-id="15bdc-159">Log out.</span></span>
 
    <span data-ttu-id="15bdc-160">Clicando o **logoff** link chamadas a `LogOut` ação.</span><span class="sxs-lookup"><span data-stu-id="15bdc-160">Clicking the **Log out** link calls the `LogOut` action.</span></span>
 
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]
 
    <span data-ttu-id="15bdc-161">O código anterior acima chama o `_signInManager.SignOutAsync` método.</span><span class="sxs-lookup"><span data-stu-id="15bdc-161">The preceding code above calls the `_signInManager.SignOutAsync` method.</span></span> <span data-ttu-id="15bdc-162">O `SignOutAsync` método limpa declarações do usuário armazenadas em um cookie.</span><span class="sxs-lookup"><span data-stu-id="15bdc-162">The `SignOutAsync` method clears the user's claims stored in a cookie.</span></span>
 
6.  <span data-ttu-id="15bdc-163">Configuração.</span><span class="sxs-lookup"><span data-stu-id="15bdc-163">Configuration.</span></span>

    <span data-ttu-id="15bdc-164">Identidade tem alguns comportamentos padrão que você pode substituir na classe de inicialização do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="15bdc-164">Identity has some default behaviors that you can override in your application's startup class.</span></span> <span data-ttu-id="15bdc-165">Você não precisa configurar ``IdentityOptions`` se você estiver usando os comportamentos padrão.</span><span class="sxs-lookup"><span data-stu-id="15bdc-165">You do not need to configure ``IdentityOptions`` if you are using the default behaviors.</span></span>

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="15bdc-166">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="15bdc-166">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-39)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="15bdc-167">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="15bdc-167">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=13-34)]

    ---
    
    <span data-ttu-id="15bdc-168">Para obter mais informações sobre como configurar a identidade, consulte [configurar identidade](xref:security/authentication/identity-configuration).</span><span class="sxs-lookup"><span data-stu-id="15bdc-168">For more information about how to configure Identity, see [Configure Identity](xref:security/authentication/identity-configuration).</span></span>
    
    <span data-ttu-id="15bdc-169">Você também pode configurar o tipo de dados da chave primária, consulte [tipo de dados de chaves primárias de configurar identidade](xref:security/authentication/identity-primary-key-configuration).</span><span class="sxs-lookup"><span data-stu-id="15bdc-169">You also can configure the data type of the primary key, see [Configure Identity primary keys data type](xref:security/authentication/identity-primary-key-configuration).</span></span>
 
7.  <span data-ttu-id="15bdc-170">Exiba o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="15bdc-170">View the database.</span></span>

    <span data-ttu-id="15bdc-171">Se seu aplicativo estiver usando um banco de dados do SQL Server (o padrão no Windows e para usuários do Visual Studio), você pode exibir o aplicativo que criou o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="15bdc-171">If your app is using a SQL Server database (the default on Windows and for Visual Studio users), you can view the database the app created.</span></span> <span data-ttu-id="15bdc-172">Você pode usar **SQL Server Management Studio**.</span><span class="sxs-lookup"><span data-stu-id="15bdc-172">You can use **SQL Server Management Studio**.</span></span> <span data-ttu-id="15bdc-173">Como alternativa, no Visual Studio, selecione **exibição** -> **Pesquisador de objetos do SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="15bdc-173">Alternatively, from Visual Studio, select **View** -> **SQL Server Object Explorer**.</span></span> <span data-ttu-id="15bdc-174">Conecte-se ao **(localdb) \MSSQLLocalDB**.</span><span class="sxs-lookup"><span data-stu-id="15bdc-174">Connect to **(localdb)\MSSQLLocalDB**.</span></span> <span data-ttu-id="15bdc-175">O banco de dados com um nome correspondente  **aspnet - <*nome do projeto*>-<*cadeia de caracteres de data*> * * é exibida.</span><span class="sxs-lookup"><span data-stu-id="15bdc-175">The database with a name matching **aspnet-<*name of your project*>-<*date string*>** is displayed.</span></span>

    ![Menu de contexto no AspNetUsers tabela de banco de dados](identity/_static/04-db.png)
    
    <span data-ttu-id="15bdc-177">Expanda o banco de dados e sua **tabelas**, clique com o **dbo. AspNetUsers** de tabela e selecione **exibir dados**.</span><span class="sxs-lookup"><span data-stu-id="15bdc-177">Expand the database and its **Tables**, then right-click the **dbo.AspNetUsers** table and select **View Data**.</span></span>

8. <span data-ttu-id="15bdc-178">Verifique se funciona de identidade</span><span class="sxs-lookup"><span data-stu-id="15bdc-178">Verify Identity works</span></span>

    <span data-ttu-id="15bdc-179">O padrão *aplicativo Web do ASP.NET Core* modelo de projeto permite que os usuários acessem qualquer ação no aplicativo sem precisar para fazer logon.</span><span class="sxs-lookup"><span data-stu-id="15bdc-179">The default *ASP.NET Core Web Application* project template allows users to access any action in the application without having to login.</span></span> <span data-ttu-id="15bdc-180">Para verificar se a identidade do ASP.NET funciona, adicione um`[Authorize]` de atributo para o `About` ação do `Home` controlador.</span><span class="sxs-lookup"><span data-stu-id="15bdc-180">To verify that ASP.NET Identity works, add an`[Authorize]` attribute to the `About` action of the `Home` Controller.</span></span>
 
    ```cs
    [Authorize]
    public IActionResult About()
    {
        ViewData["Message"] = "Your application description page.";
        return View();
    }
    ```
    
    # <a name="visual-studiotabvisualstudio"></a>[<span data-ttu-id="15bdc-181">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="15bdc-181">Visual Studio</span></span>](#tab/visualstudio)     

    <span data-ttu-id="15bdc-182">Execute o projeto usando **Ctrl** + **F5** e navegue até o **sobre** página.</span><span class="sxs-lookup"><span data-stu-id="15bdc-182">Run the project using **Ctrl** + **F5** and navigate to the **About** page.</span></span> <span data-ttu-id="15bdc-183">Somente usuários autenticados podem acessar o **sobre** página agora, para que o ASP.NET redireciona para a página de logon para efetuar login ou registrar.</span><span class="sxs-lookup"><span data-stu-id="15bdc-183">Only authenticated users may access the **About** page now, so ASP.NET redirects you to the login page to login or register.</span></span>

    # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="15bdc-184">CLI do .NET Core</span><span class="sxs-lookup"><span data-stu-id="15bdc-184">.NET Core CLI</span></span>](#tab/netcore-cli)

    <span data-ttu-id="15bdc-185">Abra uma janela de comando e navegue para a raiz do projeto diretório que contém o `.csproj` arquivo.</span><span class="sxs-lookup"><span data-stu-id="15bdc-185">Open a command window and navigate to the project's root directory containing the `.csproj` file.</span></span> <span data-ttu-id="15bdc-186">Execute o `dotnet run` comando para executar o aplicativo:</span><span class="sxs-lookup"><span data-stu-id="15bdc-186">Run the `dotnet run` command to run the app:</span></span>

    ```cs
    dotnet run 
    ```

    <span data-ttu-id="15bdc-187">Procurar a URL especificada na saída de `dotnet run` comando.</span><span class="sxs-lookup"><span data-stu-id="15bdc-187">Browse the URL specified in the output from the `dotnet run` command.</span></span> <span data-ttu-id="15bdc-188">A URL deve apontar para `localhost` com um número de porta gerado.</span><span class="sxs-lookup"><span data-stu-id="15bdc-188">The URL should point to `localhost` with a generated port number.</span></span> <span data-ttu-id="15bdc-189">Navegue até o **sobre** página.</span><span class="sxs-lookup"><span data-stu-id="15bdc-189">Navigate to the **About** page.</span></span> <span data-ttu-id="15bdc-190">Somente usuários autenticados podem acessar o **sobre** página agora, para que o ASP.NET redireciona para a página de logon para efetuar login ou registrar.</span><span class="sxs-lookup"><span data-stu-id="15bdc-190">Only authenticated users may access the **About** page now, so ASP.NET redirects you to the login page to login or register.</span></span>

    ---

## <a name="identity-components"></a><span data-ttu-id="15bdc-191">Componentes de identidade</span><span class="sxs-lookup"><span data-stu-id="15bdc-191">Identity Components</span></span>

<span data-ttu-id="15bdc-192">O assembly de referência principal para o sistema de identidade é `Microsoft.AspNetCore.Identity`.</span><span class="sxs-lookup"><span data-stu-id="15bdc-192">The primary reference assembly for the Identity system is `Microsoft.AspNetCore.Identity`.</span></span> <span data-ttu-id="15bdc-193">Este pacote contém o conjunto principal de interfaces para a identidade do ASP.NET Core e está incluído por `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="15bdc-193">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

<span data-ttu-id="15bdc-194">Essas dependências são necessárias para usar o sistema de identidade em aplicativos do ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="15bdc-194">These dependencies are needed to use the Identity system in ASP.NET Core applications:</span></span>

* <span data-ttu-id="15bdc-195">`Microsoft.AspNetCore.Identity.EntityFrameworkCore`-Contém os tipos necessários para usar a identidade com o Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="15bdc-195">`Microsoft.AspNetCore.Identity.EntityFrameworkCore` - Contains the required types to use Identity with Entity Framework Core.</span></span>

* <span data-ttu-id="15bdc-196">`Microsoft.EntityFrameworkCore.SqlServer`-Entity Framework Core é uma tecnologia de acesso a dados recomendada da Microsoft para bancos de dados relacionais, como o SQL Server.</span><span class="sxs-lookup"><span data-stu-id="15bdc-196">`Microsoft.EntityFrameworkCore.SqlServer` - Entity Framework Core is Microsoft's recommended data access technology for relational databases like SQL Server.</span></span> <span data-ttu-id="15bdc-197">Para testar, você pode usar `Microsoft.EntityFrameworkCore.InMemory`.</span><span class="sxs-lookup"><span data-stu-id="15bdc-197">For testing, you can use `Microsoft.EntityFrameworkCore.InMemory`.</span></span>

* <span data-ttu-id="15bdc-198">`Microsoft.AspNetCore.Authentication.Cookies`-Middleware que permite que um aplicativo usar a autenticação baseada em cookie.</span><span class="sxs-lookup"><span data-stu-id="15bdc-198">`Microsoft.AspNetCore.Authentication.Cookies` - Middleware that enables an app to use cookie-based authentication.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="15bdc-199">Migrar a identidade do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="15bdc-199">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="15bdc-200">Para obter informações adicionais e diretrizes sobre como migrar sua identidade existente store consulte [migrando autenticação e identidade](xref:migration/identity).</span><span class="sxs-lookup"><span data-stu-id="15bdc-200">For additional information and guidance on migrating your existing Identity store see [Migrating Authentication and Identity](xref:migration/identity).</span></span>

## <a name="next-steps"></a><span data-ttu-id="15bdc-201">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="15bdc-201">Next Steps</span></span>

* [<span data-ttu-id="15bdc-202">Migrando autenticação e identidade</span><span class="sxs-lookup"><span data-stu-id="15bdc-202">Migrating Authentication and Identity</span></span>](xref:migration/identity)
* [<span data-ttu-id="15bdc-203">Confirmação de conta e recuperação de senha</span><span class="sxs-lookup"><span data-stu-id="15bdc-203">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="15bdc-204">Autenticação de dois fatores com SMS</span><span class="sxs-lookup"><span data-stu-id="15bdc-204">Two-factor authentication with SMS</span></span>](xref:security/authentication/2fa)
* [<span data-ttu-id="15bdc-205">Habilitando a autenticação usando o Facebook, o Google e outros provedores externos</span><span class="sxs-lookup"><span data-stu-id="15bdc-205">Enabling authentication using Facebook, Google and other external providers</span></span>](xref:security/authentication/social/index)
