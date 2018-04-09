---
title: Introdução à identidade do ASP.NET Core
author: rick-anderson
description: Use identidade com um aplicativo do ASP.NET Core. Inclui a configuração de senha (RequireDigit, RequiredLength, RequiredUniqueChars e muito mais).
manager: wpickett
ms.author: riande
ms.date: 01/24/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/identity
ms.openlocfilehash: b3bfae665403162db1fb012fac227275b1dfd6c9
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
# <a name="introduction-to-identity-on-aspnet-core"></a>Introdução à identidade do ASP.NET Core

Por [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), Jon Galloway [Erik Reitan](https://github.com/Erikre), e [Steve Smith](https://ardalis.com/)

A identidade do ASP.NET Core é um sistema de associação que permite que você adicione a funcionalidade de login ao seu aplicativo. Os usuários podem criar uma conta e um logon com nome de usuário e senha ou usar um provedor de logon externo, como Facebook, Google, Microsoft Account, Twitter ou outros.

Você pode configurar a identidade do ASP.NET Core para usar um banco de dados do SQL Server para armazenar os nomes de usuário, senhas e dados de perfil. Como alternativa, você pode usar seu próprio armazenamento persistente, por exemplo, um armazenamento de tabela do Azure. Este documento contém instruções para Visual Studio e usando a CLI.

[Exibir ou baixar o código de exemplo.](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) [(Como fazer o download)](https://docs.microsoft.com/aspnet/core/tutorials/index#how-to-download-a-sample)

## <a name="overview-of-identity"></a>Visão geral da identidade

Neste tópico, você vai aprender a usar a identidade do ASP.NET Core para adicionar a funcionalidade para se registrar, fazer logon e fazer logoff de um usuário. Para obter instruções mais detalhadas sobre a criação de aplicativos usando a identidade do ASP.NET Core, consulte a seção Próximas etapas no final deste artigo.

1. Crie um projeto de aplicativo Web do ASP.NET Core com contas de usuário individuais.

   # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

   No Visual Studio, selecione **arquivo** > **novo** > **projeto**. Selecione **aplicativo Web do ASP.NET Core** e clique em **Okey**.

   ![Caixa de diálogo Novo Projeto](identity/_static/01-new-project.png)

   Selecione um ASP.NET Core **aplicativo Web (Model-View-Controller)** do ASP.NET Core 2. x, em seguida, selecione **alterar autenticação**.

   ![Caixa de diálogo Novo Projeto](identity/_static/02-new-project.png)

   Oferta de será exibida uma caixa de diálogo Opções de autenticação. Selecione **contas de usuário individuais** e clique em **Okey** para retornar à caixa de diálogo anterior.

   ![Caixa de diálogo Novo Projeto](identity/_static/03-new-project-auth.png)

   Selecionando **contas de usuário individuais** direciona o Visual Studio para criar modelos, ViewModels, modos de exibição, controladores e outros ativos necessários para a autenticação como parte do modelo de projeto.

   # <a name="net-core-clitabnetcore-cli"></a>[CLI do .NET Core](#tab/netcore-cli)

   Se usar o .NET Core CLI, crie o novo projeto usando ``dotnet new mvc --auth Individual``. Este comando cria um novo projeto com o mesmo código de modelo de identidade que Visual Studio cria.

   O projeto criado contém o `Microsoft.AspNetCore.Identity.EntityFrameworkCore` pacote, persiste os dados de identidade e o esquema para o SQL Server usando [Entity Framework Core](https://docs.microsoft.com/ef/).

   ---

2. Configurar os serviços de identidade e adicionar middleware em `Startup`.

   Os serviços de identidade são adicionados ao aplicativo o `ConfigureServices` método o `Startup` classe:

   #### <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)
   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   Esses serviços são disponibilizados para o aplicativo por meio de [injeção de dependência](xref:fundamentals/dependency-injection).

   Identidade é habilitada para o aplicativo chamando `UseAuthentication` no `Configure` método. `UseAuthentication` Adiciona autenticação [middleware](xref:fundamentals/middleware/index) para o pipeline de solicitação.

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]

   #### <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)
   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-33)]

   Esses serviços são disponibilizados para o aplicativo por meio de [injeção de dependência](xref:fundamentals/dependency-injection).

   Identidade é habilitada para o aplicativo chamando `UseIdentity` no `Configure` método. `UseIdentity` Adiciona a autenticação baseada em cookie [middleware](xref:fundamentals/middleware/index) para o pipeline de solicitação.

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]

   * * *
   Para obter mais informações sobre o processo de inicialização do aplicativo, consulte [inicialização do aplicativo](xref:fundamentals/startup).

3. Crie um usuário.

   Inicie o aplicativo e, em seguida, clique no link **Registrar**.

   Se esta for a primeira vez em que você estiver executando essa ação, talvez seja necessário para executar migrações. O aplicativo solicita que você **migrações aplicar**. Se necessário, atualize a página.

   ![Aplicar página da Web de migrações](identity/_static/apply-migrations.png)

   Como alternativa, você pode testar usando a identidade do ASP.NET Core com seu aplicativo sem um banco de dados persistente usando um banco de dados na memória. Para usar um banco de dados na memória, adicione o ``Microsoft.EntityFrameworkCore.InMemory`` do pacote para seu aplicativo e modifique a chamada do aplicativo para ``AddDbContext`` na ``ConfigureServices`` da seguinte maneira:

   ```csharp
   services.AddDbContext<ApplicationDbContext>(options =>
       options.UseInMemoryDatabase(Guid.NewGuid().ToString()));
   ```

   Quando o usuário clica o **registrar** link, o ``Register`` ação é invocada no ``AccountController``. O ``Register`` ação cria o usuário chamando `CreateAsync` no `_userManager` objeto (fornecido para ``AccountController`` por injeção de dependência):

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

   Se o usuário foi criado com êxito, o usuário é conectado pela chamada para ``_signInManager.SignInAsync``.

   **Observação:** consulte a [confirmação de conta](xref:security/authentication/accconfirm#prevent-login-at-registration) para verificar as etapas para impedir o logon imediato no registro.

4. Iniciar sessão.

   Os usuários podem entrar clicando no **login**, no link na parte superior do site, ou podem ser direcionados para a página de login se tentarem acessar uma parte do site que requer autorização. Quando o usuário envia o formulário na página de login, a ação``AccountController`` ``Login`` é chamada.

   O ``Login`` faz chamadas ``PasswordSignInAsync`` no objeto ``_signInManager`` (fornecido pelo ``AccountController`` por injeção de dependência).

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]

   A base de ``Controller`` classe expõe um ``User`` propriedade que você pode acessar de métodos do controlador. Por exemplo, você pode enumerar `User.Claims` e tomar decisões de autorização. Para obter mais informações, consulte [autorização](xref:security/authorization/index).

5. Faça logoff.

   Clicando o **logoff** link chamadas a `LogOut` ação.

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]

   O código anterior acima chama o `_signInManager.SignOutAsync` método. O `SignOutAsync` método limpa declarações do usuário armazenadas em um cookie.

<a name="pw"></a>
6. Configuração.

   Identidade tem alguns comportamentos padrão que podem ser substituídos na classe de inicialização do aplicativo. `IdentityOptions` não precisa ser configurado ao usar os comportamentos padrão. O código a seguir define várias opções de força de senha:

   #### <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)
   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   #### <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)
   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=13-33)]

   * * *
   Para obter mais informações sobre como configurar a identidade, consulte [configurar identidade](xref:security/authentication/identity-configuration).

   Você também pode configurar o tipo de dados da chave primária, consulte [tipo de dados de chaves primárias de configurar identidade](xref:security/authentication/identity-primary-key-configuration).

7. Exiba o banco de dados.

   Se o seu aplicativo estiver usando um banco de dados do SQL Server (o padrão no Windows e para usuários do Visual Studio), você poderá exibir o banco de dados criado pelo aplicativo. Você pode usar **SQL Server Management Studio**. Como alternativa, no Visual Studio, selecione **exibição** > **Pesquisador de objetos do SQL Server**. Conecte-se ao **(localdb) \MSSQLLocalDB**. O banco de dados com um nome correspondente **aspnet - <*nome do projeto*>-<*cadeia de caracteres de data* >**  é exibido.

   ![Menu de contexto no AspNetUsers tabela de banco de dados](identity/_static/04-db.png)

   Expanda o banco de dados e sua **tabelas**, clique com o **dbo. AspNetUsers** de tabela e selecione **exibir dados**.

8. Verifique se a identidade está funcionando

    O modelo de projeto padrão *aplicativo Web do ASP.NET Core* permite que os usuários acessem qualquer ação no aplicativo sem precisar fazer logon. Para verificar se a identidade do ASP.NET funciona, adicione um`[Authorize]` como atributo para a ação `About` do controlador `Home`.

    ```cs
    [Authorize]
    public IActionResult About()
    {
        ViewData["Message"] = "Your application description page.";
        return View();
    }
    ```

    # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

    Execute o projeto usando **Ctrl** + **F5** e navegue até o **sobre** página. Somente usuários autenticados podem acessar o **sobre** página agora, para que o ASP.NET redireciona para a página de logon para efetuar login ou registrar.

    # <a name="net-core-clitabnetcore-cli"></a>[CLI do .NET Core](#tab/netcore-cli)

    Abra uma janela de comando e navegue para a raiz do projeto diretório que contém o `.csproj` arquivo. Execute o [dotnet execute](/dotnet/core/tools/dotnet-run) comando para executar o aplicativo:

    ```cs
    dotnet run 
    ```

    Procurar a URL especificada na saída de [dotnet execute](/dotnet/core/tools/dotnet-run) comando. A URL deve apontar para `localhost` com um número de porta gerado. Navegue até o **sobre** página. Somente usuários autenticados podem acessar o **sobre** página agora, para que o ASP.NET redireciona para a página de logon para efetuar login ou registrar.

    ---

## <a name="identity-components"></a>Componentes de identidade

O assembly de referência principal para o sistema de identidade é `Microsoft.AspNetCore.Identity`. Este pacote contém o conjunto principal de interfaces para a identidade do ASP.NET Core e está incluído por `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.

Essas dependências são necessárias para usar o sistema de identidade em aplicativos do ASP.NET Core:

* `Microsoft.AspNetCore.Identity.EntityFrameworkCore` -Contém os tipos necessários para usar a identidade com o Entity Framework Core.

* `Microsoft.EntityFrameworkCore.SqlServer` -Entity Framework Core é uma tecnologia de acesso a dados recomendada da Microsoft para bancos de dados relacionais, como o SQL Server. Para testar, você pode usar `Microsoft.EntityFrameworkCore.InMemory`.

* `Microsoft.AspNetCore.Authentication.Cookies` -Middleware que permite que um aplicativo usar a autenticação baseada em cookie.

## <a name="migrating-to-aspnet-core-identity"></a>Migrar a identidade do ASP.NET Core

Para obter informações adicionais e diretrizes sobre como migrar sua identidade existente store consulte [migrar autenticação e identidade](xref:migration/identity).

## <a name="setting-password-strength"></a>Definindo o nível de senha

Consulte [configuração](#pw) para obter um exemplo que defina os requisitos mínimos de senha.

## <a name="next-steps"></a>Próximas etapas

* [Migrar de autenticação e identidade](xref:migration/identity)
* [Confirmação de conta e recuperação de senha](xref:security/authentication/accconfirm)
* [Autenticação de dois fatores com SMS](xref:security/authentication/2fa)
* [Facebook, Google e autenticação do provedor externo](xref:security/authentication/social/index)
