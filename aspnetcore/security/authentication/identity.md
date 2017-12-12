---
title: "Introdução à identidade no núcleo do ASP.NET"
author: rick-anderson
description: Usando a identidade com um aplicativo do ASP.NET Core
keywords: "Autorização de ASP.NET Core, identidade, segurança"
ms.author: riande
manager: wpickett
ms.date: 07/07/2017
ms.topic: article
ms.assetid: cf119f21-1a2b-49a2-b052-547ccb66ee83
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity
ms.openlocfilehash: 0679663b3b3b66f9935d0fb24360be2954fcdee1
ms.sourcegitcommit: 9ecd4e9fb0c40c3693dab079eab1ff94b461c922
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2017
---
# <a name="introduction-to-identity-on-aspnet-core"></a><span data-ttu-id="20a64-104">Introdução à identidade no núcleo do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="20a64-104">Introduction to Identity on ASP.NET Core</span></span>

<span data-ttu-id="20a64-105">Por [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), Jon Galloway [Erik Reitan](https://github.com/Erikre), e [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="20a64-105">By [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), Jon Galloway, [Erik Reitan](https://github.com/Erikre), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="20a64-106">Identidade do ASP.NET Core é um sistema de associação que permite que você adicionar a funcionalidade de logon ao seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="20a64-106">ASP.NET Core Identity is a membership system which allows you to add login functionality to your application.</span></span> <span data-ttu-id="20a64-107">Os usuários podem criar uma conta e logon com um nome de usuário e senha ou eles podem usar um provedor de logon externo, como Facebook, Google, Microsoft Account, Twitter ou outras pessoas.</span><span class="sxs-lookup"><span data-stu-id="20a64-107">Users can create an account and login with a user name and password or they can use an external login provider such as Facebook, Google, Microsoft Account, Twitter or others.</span></span>

<span data-ttu-id="20a64-108">Você pode configurar a identidade do ASP.NET Core para usar um banco de dados do SQL Server para armazenar os nomes de usuário, senhas e dados de perfil.</span><span class="sxs-lookup"><span data-stu-id="20a64-108">You can configure ASP.NET Core Identity to use a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="20a64-109">Como alternativa, você pode usar seu próprio armazenamento persistente, por exemplo o armazenamento de tabela do Azure.</span><span class="sxs-lookup"><span data-stu-id="20a64-109">Alternatively, you can use your own persistent store, for example Azure Table Storage.</span></span> <span data-ttu-id="20a64-110">Este documento contém instruções para Visual Studio e usando a CLI.</span><span class="sxs-lookup"><span data-stu-id="20a64-110">This document contains instructions for Visual Studio and for using the CLI.</span></span>

## <a name="overview-of-identity"></a><span data-ttu-id="20a64-111">Visão geral da identidade</span><span class="sxs-lookup"><span data-stu-id="20a64-111">Overview of Identity</span></span>

<span data-ttu-id="20a64-112">Neste tópico, você vai aprender a usar a identidade do ASP.NET Core para adicionar funcionalidade ao se registrar, faça logon no e fazer logoff de um usuário.</span><span class="sxs-lookup"><span data-stu-id="20a64-112">In this topic, you'll learn how to use ASP.NET Core Identity to add functionality to register, log in, and log out a user.</span></span> <span data-ttu-id="20a64-113">Para obter instruções mais detalhadas sobre a criação de aplicativos usando a identidade do ASP.NET Core, consulte a seção próximas etapas no final deste artigo.</span><span class="sxs-lookup"><span data-stu-id="20a64-113">For more detailed instructions about creating apps using ASP.NET Core Identity, see the Next Steps section at the end of this article.</span></span>

1.  <span data-ttu-id="20a64-114">Crie um projeto de aplicativo Web do ASP.NET Core com contas de usuário individuais.</span><span class="sxs-lookup"><span data-stu-id="20a64-114">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

    # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="20a64-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="20a64-115">Visual Studio</span></span>](#tab/visual-studio)
    <span data-ttu-id="20a64-116">No Visual Studio, selecione **arquivo** -> **novo** -> **projeto**.</span><span class="sxs-lookup"><span data-stu-id="20a64-116">In Visual Studio, select **File** -> **New** -> **Project**.</span></span> <span data-ttu-id="20a64-117">Selecione o **aplicativo Web ASP.NET** do **novo projeto** caixa de diálogo.</span><span class="sxs-lookup"><span data-stu-id="20a64-117">Select the **ASP.NET Web Application** from the **New Project** dialog box.</span></span> <span data-ttu-id="20a64-118">Selecionando um ASP.NET Core **Web Application(Model-View-Controller)** para ASP.NET Core 2. x com **contas de usuário individuais** como o método de autenticação.</span><span class="sxs-lookup"><span data-stu-id="20a64-118">Selecting an ASP.NET Core **Web Application(Model-View-Controller)** for ASP.NET Core 2.x with **Individual User Accounts** as the authentication method.</span></span>

    <span data-ttu-id="20a64-119">Observação: Você deve selecionar **contas de usuário individuais**.</span><span class="sxs-lookup"><span data-stu-id="20a64-119">Note: You must select **Individual User Accounts**.</span></span>
 
    ![Caixa de diálogo Novo Projeto](identity/_static/01-mvc_2.png)
    
    # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="20a64-121">CLI do .NET Core</span><span class="sxs-lookup"><span data-stu-id="20a64-121">.NET Core CLI</span></span>](#tab/netcore-cli)
    <span data-ttu-id="20a64-122">Se usar o .NET Core CLI, crie o novo projeto usando ``dotnet new mvc --auth Individual``.</span><span class="sxs-lookup"><span data-stu-id="20a64-122">If using the .NET Core CLI, create the new project using ``dotnet new mvc --auth Individual``.</span></span> <span data-ttu-id="20a64-123">Isso criará um novo projeto com o mesmo código de modelo de identidade que Visual Studio cria.</span><span class="sxs-lookup"><span data-stu-id="20a64-123">This will create a new project with the same Identity template code Visual Studio creates.</span></span>
 
    <span data-ttu-id="20a64-124">O projeto criado contém o `Microsoft.AspNetCore.Identity.EntityFrameworkCore` pacote, que será mantido os dados de identidade e o esquema para o SQL Server usando [Entity Framework Core](https://docs.microsoft.com/ef/).</span><span class="sxs-lookup"><span data-stu-id="20a64-124">The created project contains the `Microsoft.AspNetCore.Identity.EntityFrameworkCore` package, which will persist the Identity data and schema to SQL Server using [Entity Framework Core](https://docs.microsoft.com/ef/).</span></span>
    
    ---
 
2.  <span data-ttu-id="20a64-125">Configurar os serviços de identidade e adicionar middleware em `Startup`.</span><span class="sxs-lookup"><span data-stu-id="20a64-125">Configure Identity services and add middleware in `Startup`.</span></span>

    <span data-ttu-id="20a64-126">Os serviços de identidade são adicionados ao aplicativo o `ConfigureServices` método o `Startup` classe:</span><span class="sxs-lookup"><span data-stu-id="20a64-126">The Identity services are added to the application in the `ConfigureServices` method in the `Startup` class:</span></span>

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="20a64-127">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="20a64-127">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-39)]
    
    <span data-ttu-id="20a64-128">Esses serviços são disponibilizados para o aplicativo por meio de [injeção de dependência](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="20a64-128">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>
    
    <span data-ttu-id="20a64-129">Identidade é habilitada para o aplicativo chamando `UseAuthentication` no `Configure` método.</span><span class="sxs-lookup"><span data-stu-id="20a64-129">Identity is enabled for the application by calling `UseAuthentication` in the `Configure` method.</span></span> <span data-ttu-id="20a64-130">`UseAuthentication`Adiciona autenticação [middleware](xref:fundamentals/middleware) para o pipeline de solicitação.</span><span class="sxs-lookup"><span data-stu-id="20a64-130">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware) to the request pipeline.</span></span>
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="20a64-131">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="20a64-131">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-34)]
    
    <span data-ttu-id="20a64-132">Esses serviços são disponibilizados para o aplicativo por meio de [injeção de dependência](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="20a64-132">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>
    
    <span data-ttu-id="20a64-133">Identidade é habilitada para o aplicativo chamando `UseIdentity` no `Configure` método.</span><span class="sxs-lookup"><span data-stu-id="20a64-133">Identity is enabled for the application by calling `UseIdentity` in the `Configure` method.</span></span> <span data-ttu-id="20a64-134">`UseIdentity`Adiciona a autenticação baseada em cookie [middleware](xref:fundamentals/middleware) para o pipeline de solicitação.</span><span class="sxs-lookup"><span data-stu-id="20a64-134">`UseIdentity` adds cookie-based authentication [middleware](xref:fundamentals/middleware) to the request pipeline.</span></span>
        
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]
    
    ---
     
    <span data-ttu-id="20a64-135">Para obter mais informações sobre o processo de inicialização do aplicativo, consulte [inicialização do aplicativo](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="20a64-135">For more information about the application start up process, see [Application Startup](xref:fundamentals/startup).</span></span>

3.  <span data-ttu-id="20a64-136">Crie um usuário.</span><span class="sxs-lookup"><span data-stu-id="20a64-136">Create a user.</span></span>

    <span data-ttu-id="20a64-137">Iniciar o aplicativo e, em seguida, clique no **registrar** link.</span><span class="sxs-lookup"><span data-stu-id="20a64-137">Launch the application and then click on the **Register** link.</span></span>

    <span data-ttu-id="20a64-138">Se esta for a primeira vez em que você estiver executando essa ação, talvez seja necessário para executar migrações.</span><span class="sxs-lookup"><span data-stu-id="20a64-138">If this is the first time you're performing this action, you may be required to run migrations.</span></span> <span data-ttu-id="20a64-139">O aplicativo solicita que você **aplicar migrações**:</span><span class="sxs-lookup"><span data-stu-id="20a64-139">The application prompts you to **Apply Migrations**:</span></span>
    
    ![Aplicar página da Web de migrações](identity/_static/apply-migrations.png)
    
    <span data-ttu-id="20a64-141">Como alternativa, você pode testar usando a identidade do ASP.NET Core com seu aplicativo sem um banco de dados persistente usando um banco de dados na memória.</span><span class="sxs-lookup"><span data-stu-id="20a64-141">Alternately, you can test using ASP.NET Core Identity with your app without a persistent database by using an in-memory database.</span></span> <span data-ttu-id="20a64-142">Para usar um banco de dados na memória, adicione o ``Microsoft.EntityFrameworkCore.InMemory`` do pacote para seu aplicativo e modifique a chamada do aplicativo para ``AddDbContext`` na ``ConfigureServices`` da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="20a64-142">To use an in-memory database, add the ``Microsoft.EntityFrameworkCore.InMemory`` package to your app and modify your app's call to ``AddDbContext`` in ``ConfigureServices`` as follows:</span></span>

    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseInMemoryDatabase(Guid.NewGuid().ToString()));
    ```
    
    <span data-ttu-id="20a64-143">Quando o usuário clica o **registrar** link, o ``Register`` ação é invocada no ``AccountController``.</span><span class="sxs-lookup"><span data-stu-id="20a64-143">When the user clicks the **Register** link, the ``Register`` action is invoked on ``AccountController``.</span></span> <span data-ttu-id="20a64-144">O ``Register`` ação cria o usuário chamando `CreateAsync` no `_userManager` objeto (fornecido para ``AccountController`` por injeção de dependência):</span><span class="sxs-lookup"><span data-stu-id="20a64-144">The ``Register`` action creates the user by calling `CreateAsync` on the  `_userManager` object (provided to ``AccountController`` by dependency injection):</span></span>
 
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

    <span data-ttu-id="20a64-145">Se o usuário foi criado com êxito, o usuário é conectado pela chamada para ``_signInManager.SignInAsync``.</span><span class="sxs-lookup"><span data-stu-id="20a64-145">If the user was created successfully, the user is logged in by the call to ``_signInManager.SignInAsync``.</span></span>

    <span data-ttu-id="20a64-146">**Observação:** consulte [conta confirmação](xref:security/authentication/accconfirm#prevent-login-at-registration) para obter as etapas impedir que o logon imediata no registro.</span><span class="sxs-lookup"><span data-stu-id="20a64-146">**Note:** See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>
 
4.  <span data-ttu-id="20a64-147">Iniciar sessão.</span><span class="sxs-lookup"><span data-stu-id="20a64-147">Log in.</span></span>
 
    <span data-ttu-id="20a64-148">Os usuários podem entrar clicando o **login** link na parte superior do site, ou pode ser navegados a página de logon se tentarem acessar uma parte do site que requer autorização.</span><span class="sxs-lookup"><span data-stu-id="20a64-148">Users can sign in by clicking the **Log in** link at the top of the site, or they may be navigated to the Login page if they attempt to access a part of the site that requires authorization.</span></span> <span data-ttu-id="20a64-149">Quando o usuário envia o formulário na página de logon, o ``AccountController`` ``Login`` ação é chamada.</span><span class="sxs-lookup"><span data-stu-id="20a64-149">When the user submits the form on the Login page, the ``AccountController`` ``Login`` action is called.</span></span>

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]
 
    <span data-ttu-id="20a64-150">O ``Login`` ação chamadas ``PasswordSignInAsync`` no ``_signInManager`` objeto (fornecido para ``AccountController`` por injeção de dependência).</span><span class="sxs-lookup"><span data-stu-id="20a64-150">The ``Login`` action calls ``PasswordSignInAsync`` on the ``_signInManager`` object (provided to ``AccountController`` by dependency injection).</span></span>
 
    <span data-ttu-id="20a64-151">A base de ``Controller`` classe expõe um ``User`` propriedade que você pode acessar de métodos do controlador.</span><span class="sxs-lookup"><span data-stu-id="20a64-151">The base ``Controller`` class exposes a ``User`` property that you can access from controller methods.</span></span> <span data-ttu-id="20a64-152">Por exemplo, você pode enumerar `User.Claims` e tomar decisões de autorização.</span><span class="sxs-lookup"><span data-stu-id="20a64-152">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="20a64-153">Para obter mais informações, consulte [autorização](xref:security/authorization/index).</span><span class="sxs-lookup"><span data-stu-id="20a64-153">For more information, see [Authorization](xref:security/authorization/index).</span></span>
 
5.  <span data-ttu-id="20a64-154">Faça logoff.</span><span class="sxs-lookup"><span data-stu-id="20a64-154">Log out.</span></span>
 
    <span data-ttu-id="20a64-155">Clicando o **logoff** link chamadas a `LogOut` ação.</span><span class="sxs-lookup"><span data-stu-id="20a64-155">Clicking the **Log out** link calls the `LogOut` action.</span></span>
 
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]
 
    <span data-ttu-id="20a64-156">O código anterior acima chama o `_signInManager.SignOutAsync` método.</span><span class="sxs-lookup"><span data-stu-id="20a64-156">The preceding code above calls the `_signInManager.SignOutAsync` method.</span></span> <span data-ttu-id="20a64-157">O `SignOutAsync` método limpa declarações do usuário armazenadas em um cookie.</span><span class="sxs-lookup"><span data-stu-id="20a64-157">The `SignOutAsync` method clears the user's claims stored in a cookie.</span></span>
 
6.  <span data-ttu-id="20a64-158">Configuração.</span><span class="sxs-lookup"><span data-stu-id="20a64-158">Configuration.</span></span>

    <span data-ttu-id="20a64-159">Identidade tem alguns comportamentos padrão que você pode substituir na classe de inicialização do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="20a64-159">Identity has some default behaviors that you can override in your application's startup class.</span></span> <span data-ttu-id="20a64-160">Você não precisa configurar ``IdentityOptions`` se você estiver usando os comportamentos padrão.</span><span class="sxs-lookup"><span data-stu-id="20a64-160">You do not need to configure ``IdentityOptions`` if you are using the default behaviors.</span></span>

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="20a64-161">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="20a64-161">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-39)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="20a64-162">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="20a64-162">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=13-34)]

    ---
    
    <span data-ttu-id="20a64-163">Para obter mais informações sobre como configurar a identidade, consulte [configurar identidade](xref:security/authentication/identity-configuration).</span><span class="sxs-lookup"><span data-stu-id="20a64-163">For more information about how to configure Identity, see [Configure Identity](xref:security/authentication/identity-configuration).</span></span>
    
    <span data-ttu-id="20a64-164">Você também pode configurar o tipo de dados da chave primária, consulte [tipo de dados de chaves primárias de configurar identidade](xref:security/authentication/identity-primary-key-configuration).</span><span class="sxs-lookup"><span data-stu-id="20a64-164">You also can configure the data type of the primary key, see [Configure Identity primary keys data type](xref:security/authentication/identity-primary-key-configuration).</span></span>
 
7.  <span data-ttu-id="20a64-165">Exiba o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="20a64-165">View the database.</span></span>

    <span data-ttu-id="20a64-166">Se seu aplicativo estiver usando um banco de dados do SQL Server (o padrão no Windows e para usuários do Visual Studio), você pode exibir o aplicativo que criou o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="20a64-166">If your app is using a SQL Server database (the default on Windows and for Visual Studio users), you can view the database the app created.</span></span> <span data-ttu-id="20a64-167">Você pode usar **SQL Server Management Studio**.</span><span class="sxs-lookup"><span data-stu-id="20a64-167">You can use **SQL Server Management Studio**.</span></span> <span data-ttu-id="20a64-168">Como alternativa, no Visual Studio, selecione **exibição** -> **Pesquisador de objetos do SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="20a64-168">Alternatively, from Visual Studio, select **View** -> **SQL Server Object Explorer**.</span></span> <span data-ttu-id="20a64-169">Conecte-se ao **(localdb) \MSSQLLocalDB**.</span><span class="sxs-lookup"><span data-stu-id="20a64-169">Connect to **(localdb)\MSSQLLocalDB**.</span></span> <span data-ttu-id="20a64-170">O banco de dados com um nome correspondente  **aspnet - <*nome do projeto*>-<*cadeia de caracteres de data*> * * é exibida.</span><span class="sxs-lookup"><span data-stu-id="20a64-170">The database with a name matching **aspnet-<*name of your project*>-<*date string*>** is displayed.</span></span>

    ![Menu de contexto no AspNetUsers tabela de banco de dados](identity/_static/04-db.png)
    
    <span data-ttu-id="20a64-172">Expanda o banco de dados e sua **tabelas**, clique com o **dbo. AspNetUsers** de tabela e selecione **exibir dados**.</span><span class="sxs-lookup"><span data-stu-id="20a64-172">Expand the database and its **Tables**, then right-click the **dbo.AspNetUsers** table and select **View Data**.</span></span>

## <a name="identity-components"></a><span data-ttu-id="20a64-173">Componentes de identidade</span><span class="sxs-lookup"><span data-stu-id="20a64-173">Identity Components</span></span>

<span data-ttu-id="20a64-174">O assembly de referência principal para o sistema de identidade é `Microsoft.AspNetCore.Identity`.</span><span class="sxs-lookup"><span data-stu-id="20a64-174">The primary reference assembly for the Identity system is `Microsoft.AspNetCore.Identity`.</span></span> <span data-ttu-id="20a64-175">Este pacote contém o conjunto principal de interfaces para a identidade do ASP.NET Core e está incluído por `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="20a64-175">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

<span data-ttu-id="20a64-176">Essas dependências são necessárias para usar o sistema de identidade em aplicativos do ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="20a64-176">These dependencies are needed to use the Identity system in ASP.NET Core applications:</span></span>

* <span data-ttu-id="20a64-177">`Microsoft.AspNetCore.Identity.EntityFrameworkCore`-Contém os tipos necessários para usar a identidade com o Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="20a64-177">`Microsoft.AspNetCore.Identity.EntityFrameworkCore` - Contains the required types to use Identity with Entity Framework Core.</span></span>

* <span data-ttu-id="20a64-178">`Microsoft.EntityFrameworkCore.SqlServer`-Entity Framework Core é uma tecnologia de acesso a dados recomendada da Microsoft para bancos de dados relacionais, como o SQL Server.</span><span class="sxs-lookup"><span data-stu-id="20a64-178">`Microsoft.EntityFrameworkCore.SqlServer` - Entity Framework Core is Microsoft's recommended data access technology for relational databases like SQL Server.</span></span> <span data-ttu-id="20a64-179">Para testar, você pode usar `Microsoft.EntityFrameworkCore.InMemory`.</span><span class="sxs-lookup"><span data-stu-id="20a64-179">For testing, you can use `Microsoft.EntityFrameworkCore.InMemory`.</span></span>

* <span data-ttu-id="20a64-180">`Microsoft.AspNetCore.Authentication.Cookies`-Middleware que permite que um aplicativo usar a autenticação baseada em cookie.</span><span class="sxs-lookup"><span data-stu-id="20a64-180">`Microsoft.AspNetCore.Authentication.Cookies` - Middleware that enables an app to use cookie-based authentication.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="20a64-181">Migrar a identidade do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="20a64-181">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="20a64-182">Para obter informações adicionais e diretrizes sobre como migrar sua identidade existente store consulte [migrando autenticação e identidade](xref:migration/identity).</span><span class="sxs-lookup"><span data-stu-id="20a64-182">For additional information and guidance on migrating your existing Identity store see [Migrating Authentication and Identity](xref:migration/identity).</span></span>

## <a name="next-steps"></a><span data-ttu-id="20a64-183">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="20a64-183">Next Steps</span></span>

* [<span data-ttu-id="20a64-184">Migrando autenticação e identidade</span><span class="sxs-lookup"><span data-stu-id="20a64-184">Migrating Authentication and Identity</span></span>](xref:migration/identity)
* [<span data-ttu-id="20a64-185">Confirmação de conta e recuperação de senha</span><span class="sxs-lookup"><span data-stu-id="20a64-185">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="20a64-186">Autenticação de dois fatores com SMS</span><span class="sxs-lookup"><span data-stu-id="20a64-186">Two-factor authentication with SMS</span></span>](xref:security/authentication/2fa)
* [<span data-ttu-id="20a64-187">Habilitando a autenticação usando o Facebook, o Google e outros provedores externos</span><span class="sxs-lookup"><span data-stu-id="20a64-187">Enabling authentication using Facebook, Google and other external providers</span></span>](xref:security/authentication/social/index)
