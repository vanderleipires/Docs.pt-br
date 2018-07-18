---
title: Introdução à identidade do ASP.NET Core
author: rick-anderson
description: Use identidade com um aplicativo ASP.NET Core. Inclui, definindo requisitos de senha (RequireDigit, RequiredLength, RequiredUniqueChars e muito mais).
ms.author: riande
ms.date: 01/24/2018
uid: security/authentication/identity
ms.openlocfilehash: 50ddb96000e6a3f9e1762e9bb3e1f215f20d4356
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095633"
---
# <a name="introduction-to-identity-on-aspnet-core"></a>Introdução à identidade do ASP.NET Core

Por [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), Jon Galloway [Erik Reitan](https://github.com/Erikre), e [Steve Smith](https://ardalis.com/)

A identidade do ASP.NET Core é um sistema de associação que permite que você adicione a funcionalidade de login ao seu aplicativo. Os usuários podem criar uma conta e um logon com nome de usuário e senha ou usar um provedor de logon externo, como Facebook, Google, Microsoft Account, Twitter ou outros.

Você pode configurar a identidade do ASP.NET Core para usar um banco de dados do SQL Server para armazenar nomes de usuário, senhas e dados de perfil. Como alternativa, você pode usar seu próprio armazenamento persistente, por exemplo, um armazenamento de tabela do Azure. Este documento contém instruções para o Visual Studio e usando a CLI.

[Exibir ou baixar o código de exemplo.](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) [(Como fazer o download)](xref:tutorials/index#how-to-download-a-sample)

## <a name="overview-of-identity"></a>Visão geral da identidade

Neste tópico, você vai aprender a usar a identidade do ASP.NET Core para adicionar a funcionalidade para se registrar, fazer logon e fazer logoff de um usuário. Para obter instruções mais detalhadas sobre a criação de aplicativos usando a identidade do ASP.NET Core, consulte a seção Próximas etapas no final deste artigo.

1. Crie um projeto de aplicativo Web ASP.NET Core com contas de usuário individuais.

   # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

   No Visual Studio, selecione **arquivo** > **New** > **projeto**. Selecione **aplicativo Web ASP.NET Core** e clique em **Okey**.

   ![Caixa de diálogo Novo Projeto](identity/_static/01-new-project.png)

   Selecione um ASP.NET Core **aplicativo Web (Model-View-Controller)** para o ASP.NET Core 2.x e, em seguida, selecione **alterar autenticação**.

   ![Caixa de diálogo Novo Projeto](identity/_static/02-new-project.png)

   Oferta de será exibida uma caixa de diálogo Opções de autenticação. Selecione **contas de usuário individuais** e clique em **Okey** para retornar à caixa de diálogo anterior.

   ![Caixa de diálogo Novo Projeto](identity/_static/03-new-project-auth.png)

   Selecionando **contas de usuário individuais** direciona o Visual Studio para criar modelos, ViewModels, exibições, controladores e outros ativos necessários para a autenticação como parte do modelo de projeto.

   # <a name="net-core-clitabnetcore-cli"></a>[CLI do .NET Core](#tab/netcore-cli)

   Se usando a CLI do .NET Core, crie o novo projeto usando `dotnet new mvc --auth Individual`. Este comando cria um novo projeto com o mesmo código de modelo de identidade que Visual Studio cria.

   O projeto criado contém o `Microsoft.AspNetCore.Identity.EntityFrameworkCore` pacote, que persiste os dados de identidade e o esquema para o SQL Server usando [Entity Framework Core](https://docs.microsoft.com/ef/).

   ---

2. Configure os serviços de identidade e adicione o middleware no `Startup`.

   Os serviços de identidade são adicionados ao aplicativo na `ConfigureServices` método no `Startup` classe:

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   Esses serviços são disponibilizados para o aplicativo por meio [injeção de dependência](xref:fundamentals/dependency-injection).

   Identidade é habilitada para o aplicativo, chamando `UseAuthentication` no `Configure` método. `UseAuthentication` Adiciona a autenticação [middleware](xref:fundamentals/middleware/index) ao pipeline de solicitação.

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-33)]

   Esses serviços são disponibilizados para o aplicativo por meio [injeção de dependência](xref:fundamentals/dependency-injection).

   Identidade é habilitada para o aplicativo, chamando `UseIdentity` no `Configure` método. `UseIdentity` Adiciona a autenticação baseada em cookie [middleware](xref:fundamentals/middleware/index) ao pipeline de solicitação.

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]

   ---

   Para obter mais informações sobre o processo de inicialização do aplicativo, consulte [inicialização do aplicativo](xref:fundamentals/startup).

3. Crie um usuário.

   Inicie o aplicativo e, em seguida, clique no link **Registrar**.

   Se essa for a primeira vez em que você está executando esta ação, você pode ser necessária para executar as migrações. O aplicativo solicitará que você **aplicar migrações**. Se necessário, atualize a página.

   ![Aplicar a página da Web de migrações](identity/_static/apply-migrations.png)

   Como alternativa, você pode testar usando o ASP.NET Core Identity com seu aplicativo sem um banco de dados persistente usando um banco de dados na memória. Para usar um banco de dados na memória, adicione a `Microsoft.EntityFrameworkCore.InMemory` de pacote ao seu aplicativo e modifique a chamada do seu aplicativo para `AddDbContext` no `ConfigureServices` da seguinte maneira:

   ```csharp
   services.AddDbContext<ApplicationDbContext>(options =>
       options.UseInMemoryDatabase(Guid.NewGuid().ToString()));
   ```

   Quando o usuário clica o **registre** link, o `Register` ação é invocada em `AccountController`. O `Register` ação cria o usuário chamando `CreateAsync` sobre o `_userManager` objeto (fornecidas para `AccountController` pela injeção de dependência):

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

   Se o usuário foi criado com êxito, o usuário é conectado pela chamada para `_signInManager.SignInAsync`.

   **Observação:** consulte a [confirmação de conta](xref:security/authentication/accconfirm#prevent-login-at-registration) para verificar as etapas para impedir o logon imediato no registro.

4. Iniciar sessão.

   Os usuários podem entrar clicando no **login**, no link na parte superior do site, ou podem ser direcionados para a página de login se tentarem acessar uma parte do site que requer autorização. Quando o usuário envia o formulário na página de login, a ação`AccountController` `Login` é chamada.

   O `Login` faz chamadas `PasswordSignInAsync` no objeto `_signInManager` (fornecido pelo `AccountController` por injeção de dependência).

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]

   A base `Controller` classe expõe um `User` propriedade que você pode acessar métodos do controlador. Por exemplo, você pode enumerar `User.Claims` e tomar decisões de autorização. Para obter mais informações, consulte [autorização](xref:security/authorization/index).

5. Faça logoff.

   Clicar a **fazer logoff** vincular chamadas a `LogOut` ação.

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]

   O código anterior acima chamadas a `_signInManager.SignOutAsync` método. O `SignOutAsync` método limpa as declarações do usuário armazenadas em um cookie.

<a name="pw"></a>
6. Configuração.

   A identidade tem alguns comportamentos padrão que podem ser substituídos na classe de inicialização do aplicativo. `IdentityOptions` não precisa ser configurado ao usar os comportamentos padrão. O código a seguir define várias opções de força de senha:

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=13-33)]

   ---

   Para obter mais informações sobre como configurar a identidade, consulte [configurar identidade](xref:security/authentication/identity-configuration).

   Você também pode configurar o tipo de dados da chave primária, consulte [tipo de dados de chaves primárias de configurar a identidade](xref:security/authentication/identity-primary-key-configuration).

7. Exiba o banco de dados.

   Se o seu aplicativo estiver usando um banco de dados do SQL Server (o padrão no Windows e para usuários do Visual Studio), você poderá exibir o banco de dados criado pelo aplicativo. Você pode usar **SQL Server Management Studio**. Como alternativa, no Visual Studio, selecione **exibição** > **Pesquisador de objetos do SQL Server**. Conecte-se ao **(localdb) \MSSQLLocalDB**. O banco de dados com um nome que corresponda `aspnet-<name of your project>-<guid>` é exibida.

   ![Menu de contexto no AspNetUsers tabela de banco de dados](identity/_static/04-db.png)

   Expanda o banco de dados e sua **tabelas**, em seguida, clique com botão direito do **dbo. AspNetUsers** de tabela e selecione **exibir dados**.

8. Verifique se a identidade está funcionando

    O modelo de projeto padrão *aplicativo Web do ASP.NET Core* permite que os usuários acessem qualquer ação no aplicativo sem precisar fazer logon. Para verificar se a identidade do ASP.NET funciona, adicione um`[Authorize]` como atributo para a ação `About` do controlador `Home`.

    ```csharp
    [Authorize]
    public IActionResult About()
    {
        ViewData["Message"] = "Your application description page.";
        return View();
    }
    ```

    # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

    Execute o projeto usando **Ctrl** + **F5** e navegue até a **sobre** página. Somente usuários autenticados podem acessar o **sobre** página agora, portanto, o ASP.NET redireciona você à página de logon para fazer logon ou registre-se.

    # <a name="net-core-clitabnetcore-cli"></a>[CLI do .NET Core](#tab/netcore-cli)

    Abra uma janela de comando e navegue até a raiz do projeto diretório que contém o `.csproj` arquivo. Execute o [execução dotnet](/dotnet/core/tools/dotnet-run) comando para executar o aplicativo:

    ```csharp
    dotnet run 
    ```

    Procure a URL especificada na saída do [execução dotnet](/dotnet/core/tools/dotnet-run) comando. A URL deve apontar para `localhost` com um número de porta gerado. Navegue até a **sobre** página. Somente usuários autenticados podem acessar o **sobre** página agora, portanto, o ASP.NET redireciona você à página de logon para fazer logon ou registre-se.

    ---

## <a name="identity-components"></a>Componentes de identidade

O assembly de referência principal para o sistema de identidade é `Microsoft.AspNetCore.Identity`. Este pacote contém o conjunto principal de interfaces para ASP.NET Core Identity e é incluído por `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.

Essas dependências são necessárias para usar o sistema de identidade em aplicativos ASP.NET Core:

* `Microsoft.AspNetCore.Identity.EntityFrameworkCore` -Contém os tipos necessários para usar a identidade com o Entity Framework Core.

* `Microsoft.EntityFrameworkCore.SqlServer` -Entity Framework Core é uma tecnologia de acesso de dados recomendados da Microsoft para bancos de dados relacionais como o SQL Server. Para teste, você pode usar `Microsoft.EntityFrameworkCore.InMemory`.

* `Microsoft.AspNetCore.Authentication.Cookies` -Middleware que permite que um aplicativo usar a autenticação baseada em cookie.

## <a name="migrating-to-aspnet-core-identity"></a>Migrando para o ASP.NET Core Identity

Para obter mais informações e orientações sobre como migrar sua identidade existente da store ver [migrar autenticação e identidade](xref:migration/identity).

## <a name="setting-password-strength"></a>Definindo a força da senha

Consulte [configuração](#pw) para obter um exemplo que defina os requisitos mínimos de senha.

## <a name="next-steps"></a>Próximas etapas

* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:security/authentication/social/index>
* <xref:host-and-deploy/web-farm>
