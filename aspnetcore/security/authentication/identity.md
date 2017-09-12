---
title: "Introdução à identidade no núcleo do ASP.NET"
author: rick-anderson
description: Usando a identidade com um aplicativo do ASP.NET Core
keywords: "Autorização de ASP.NET Core, identidade, segurança"
ms.author: riande
manager: wpickett
ms.date: 7/7/2017
ms.topic: article
ms.assetid: cf119f21-1a2b-49a2-b052-547ccb66ee83
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity
ms.openlocfilehash: 72802830660ddcf479e540de7cfc33a07c49dc23
ms.sourcegitcommit: b02db6da115e55140da91b67355aaf56aae1703f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/11/2017
---
# <a name="introduction-to-identity-on-aspnet-core"></a>Introdução à identidade no núcleo do ASP.NET

Por [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), Jon Galloway [Erik Reitan](https://github.com/Erikre), e [Steve Smith](http://ardalis.com)

Identidade do ASP.NET Core é um sistema de associação que permite que você adicionar a funcionalidade de logon ao seu aplicativo. Os usuários podem criar uma conta e logon com um nome de usuário e senha ou eles podem usar um provedor de logon externo, como Facebook, Google, Microsoft Account, Twitter ou outras pessoas.

Você pode configurar a identidade do ASP.NET Core para usar um banco de dados do SQL Server para armazenar os nomes de usuário, senhas e dados de perfil. Como alternativa, você pode usar seu próprio armazenamento persistente, por exemplo o armazenamento de tabela do Azure. Este documento contém instruções para Visual Studio e usando a CLI.

## <a name="overview-of-identity"></a>Visão geral da identidade

Neste tópico, você vai aprender a usar a identidade do ASP.NET Core para adicionar funcionalidade ao se registrar, faça logon no e fazer logoff de um usuário. Para obter instruções mais detalhadas sobre a criação de aplicativos usando a identidade do ASP.NET Core, consulte a seção próximas etapas no final deste artigo.

1.  Crie um projeto de aplicativo Web do ASP.NET Core com contas de usuário individuais.

    # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)
    No Visual Studio, selecione **arquivo** -> **novo** -> **projeto**. Selecione o **aplicativo Web ASP.NET** do **novo projeto** caixa de diálogo. Selecionando um ASP.NET Core **aplicativo Web** com **contas de usuário individuais** como o método de autenticação.

    Observação: Você deve selecionar **contas de usuário individuais**.
 
    ![Caixa de diálogo Nova projeto](identity/_static/01-mvc.png)
    
    # <a name="net-core-clitabnetcore-cli"></a>[CLI do .NET Core](#tab/netcore-cli)
    Se usar o .NET Core CLI, crie o novo projeto usando ``dotnet new mvc --auth Individual``. Isso criará um novo projeto com o mesmo código de modelo de identidade que Visual Studio cria.
 
    O projeto criado contém o `Microsoft.AspNetCore.Identity.EntityFrameworkCore` pacote, que será mantido os dados de identidade e o esquema para o SQL Server usando [Entity Framework Core](https://docs.efproject.net).
    
    ---
 
2.  Configurar os serviços de identidade e adicionar middleware em `Startup`.

    Os serviços de identidade são adicionados ao aplicativo o `ConfigureServices` método o `Startup` classe:

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-39)]
    
    Esses serviços são disponibilizados para o aplicativo por meio de [injeção de dependência](xref:fundamentals/dependency-injection).
    
    Identidade é habilitada para o aplicativo chamando `UseAuthentication` no `Configure` método. `UseAuthentication`Adiciona autenticação [middleware](xref:fundamentals/middleware) para o pipeline de solicitação.
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-34)]
    
    Esses serviços são disponibilizados para o aplicativo por meio de [injeção de dependência](xref:fundamentals/dependency-injection).
    
    Identidade é habilitada para o aplicativo chamando `UseIdentity` no `Configure` método. `UseIdentity`Adiciona a autenticação baseada em cookie [middleware](xref:fundamentals/middleware) para o pipeline de solicitação.
        
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]
    
    ---
     
    Para obter mais informações sobre o processo de inicialização do aplicativo, consulte [inicialização do aplicativo](xref:fundamentals/startup).

3.  Crie um usuário.

    Iniciar o aplicativo e, em seguida, clique no **registrar** link.

    Se esta for a primeira vez em que você estiver executando essa ação, talvez seja necessário para executar migrações. O aplicativo solicita que você **aplicar migrações**:
    
    ![Aplicar página da Web de migrações](identity/_static/apply-migrations.png)
    
    Como alternativa, você pode testar usando a identidade do ASP.NET Core com seu aplicativo sem um banco de dados persistente usando um banco de dados na memória. Para usar um banco de dados na memória, adicione o ``Microsoft.EntityFrameworkCore.InMemory`` do pacote para seu aplicativo e modifique a chamada do aplicativo para ``AddDbContext`` na ``ConfigureServices`` da seguinte maneira:

    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseInMemoryDatabase(Guid.NewGuid().ToString()));
    ```
    
    Quando o usuário clica o **registrar** link, o ``Register`` ação é invocada no ``AccountController``. O ``Register`` ação cria o usuário chamando `CreateAsync` no `_userManager` objeto (fornecido para ``AccountController`` por injeção de dependência):
 
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

    Se o usuário foi criado com êxito, o usuário é conectado pela chamada para ``_signInManager.SignInAsync``.

    **Observação:** consulte [conta confirmação](xref:security/authentication/accconfirm#prevent-login-at-registration) para obter as etapas impedir que o logon imediata no registro.
 
4.  Iniciar sessão.
 
    Os usuários podem entrar clicando o **login** link na parte superior do site, ou pode ser navegados a página de logon se tentarem acessar uma parte do site que requer autorização. Quando o usuário envia o formulário na página de logon, o ``AccountController`` ``Login`` ação é chamada.

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]
 
    O ``Login`` ação chamadas ``PasswordSignInAsync`` no ``_signInManager`` objeto (fornecido para ``AccountController`` por injeção de dependência).
 
    A base de ``Controller`` classe expõe um ``User`` propriedade que você pode acessar de métodos do controlador. Por exemplo, você pode enumerar `User.Claims` e tomar decisões de autorização. Para obter mais informações, consulte [autorização](xref:security/authorization/index).
 
5.  Faça logoff.
 
    Clicando o **logoff** link chamadas a `LogOut` ação.
 
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]
 
    O código anterior acima chama o `_signInManager.SignOutAsync` método. O `SignOutAsync` método limpa declarações do usuário armazenadas em um cookie.
 
6.  Configuração.

    Identidade tem alguns comportamentos padrão que você pode substituir na classe de inicialização do aplicativo. Você não precisa configurar ``IdentityOptions`` se você estiver usando os comportamentos padrão.

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-39)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=13-34)]

    ---
    
    Para obter mais informações sobre como configurar a identidade, consulte [configurar identidade](xref:security/authentication/identity-configuration).
    
    Você também pode configurar o tipo de dados da chave primária, consulte [tipo de dados de chaves primárias de configurar identidade](xref:security/authentication/identity-primary-key-configuration).
 
7.  Exiba o banco de dados.

    Se seu aplicativo estiver usando um banco de dados do SQL Server (o padrão no Windows e para usuários do Visual Studio), você pode exibir o aplicativo que criou o banco de dados. Você pode usar **SQL Server Management Studio**. Como alternativa, no Visual Studio, selecione **exibição** -> **Pesquisador de objetos do SQL Server**. Conecte-se ao **(localdb) \MSSQLLocalDB**. O banco de dados com um nome correspondente * *aspnet - <*nome do projeto*>-<*cadeia de caracteres de data*> * * é exibida.

    ![Menu de contexto no AspNetUsers tabela de banco de dados](identity/_static/04-db.png)
    
    Expanda o banco de dados e sua **tabelas**, clique com o **dbo. AspNetUsers** de tabela e selecione **exibir dados**.

## <a name="identity-components"></a>Componentes de identidade

O assembly de referência principal para o sistema de identidade é `Microsoft.AspNetCore.Identity`. Este pacote contém o conjunto principal de interfaces para a identidade do ASP.NET Core e está incluído por `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.

Essas dependências são necessárias para usar o sistema de identidade em aplicativos do ASP.NET Core:

* `Microsoft.AspNetCore.Identity.EntityFrameworkCore`-Contém os tipos necessários para usar a identidade com o Entity Framework Core.

* `Microsoft.EntityFrameworkCore.SqlServer`-Entity Framework Core é uma tecnologia de acesso a dados recomendada da Microsoft para bancos de dados relacionais, como o SQL Server. Para testar, você pode usar `Microsoft.EntityFrameworkCore.InMemory`.

* `Microsoft.AspNetCore.Authentication.Cookies`-Middleware que permite que um aplicativo usar a autenticação baseada em cookie.

## <a name="migrating-to-aspnet-core-identity"></a>Migrar a identidade do ASP.NET Core

Para obter informações adicionais e diretrizes sobre como migrar sua identidade existente store consulte [migrando autenticação e identidade](xref:migration/identity).

## <a name="next-steps"></a>Próximas etapas

* [Migrando autenticação e identidade](xref:migration/identity)
* [Confirmação de conta e de recuperação de senha](xref:security/authentication/accconfirm)
* [Autenticação de dois fatores com SMS](xref:security/authentication/2fa)
* [Habilitar a autenticação usando o Facebook, o Google e outros provedores externos](xref:security/authentication/social/index)
