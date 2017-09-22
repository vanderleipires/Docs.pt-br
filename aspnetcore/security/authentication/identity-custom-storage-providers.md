---
title: Provedores de armazenamento personalizado para a identidade do ASP.NET Core | Microsoft Docs
author: ardalis
description: Como configurar provedores de armazenamento personalizado para a identidade do ASP.NET Core.
keywords: Provedores de armazenamento personalizado do ASP.NET Core, identidade,
ms.author: riande
manager: wpickett
ms.date: 05/24/2017
ms.topic: article
ms.assetid: b2ace545-ecf6-4664-b31e-b65bd4a6b025
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity-custom-storage-providers
ms.openlocfilehash: 214343c9b964baca3c436966d39caede1a58aba2
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/22/2017
---
# <a name="custom-storage-providers-for-aspnet-core-identity"></a>Provedores de armazenamento personalizado para a identidade do ASP.NET Core

Por [Steve Smith](https://ardalis.com/)

Identidade do ASP.NET Core é um sistema extensível que permite que você criar um provedor de armazenamento personalizado e conectá-lo ao seu aplicativo. Este tópico descreve como criar um provedor de armazenamento personalizado para a identidade do ASP.NET Core. Ele aborda os conceitos importantes para criar seu próprio provedor de armazenamento, mas não é um passo a passo.

[Exibir ou baixar o exemplo do GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample).

## <a name="introduction"></a>Introdução

Por padrão, o sistema de identidade do ASP.NET Core armazena informações de usuário em um banco de dados do SQL Server usando o Entity Framework Core. Para muitos aplicativos, essa abordagem funciona bem. No entanto, é preferível usar um mecanismo de persistência diferente ou esquema de dados. Por exemplo:

* Você usa [armazenamento de tabela do Azure](https://docs.microsoft.com/azure/storage/) ou outro repositório de dados.
* As tabelas de banco de dados têm uma estrutura diferente. 
* Talvez você queira usar uma abordagem de acesso a dados diferentes, como [Dapper](https://github.com/StackExchange/Dapper). 

Em cada um desses casos, você pode escrever um provedor personalizado para o mecanismo de armazenamento e conecte esse provedor de seu aplicativo.

Identidade do ASP.NET Core está incluída nos modelos de projeto no Visual Studio com a opção de "Contas de usuário individuais".

Ao usar o .NET Core CLI, adicionar `-au Individual`:

```
dotnet new mvc -au Individual
dotnet new webapi -au Individual
```

## <a name="the-aspnet-core-identity-architecture"></a>A arquitetura de identidade do ASP.NET Core

A identidade do ASP.NET Core consiste em classes chamadas gerentes e repositórios. *Gerenciadores de* classes de alto nível que um desenvolvedor de aplicativo usa para executar operações, como a criação de um usuário de identidade. *Repositórios* classes de nível inferior que especificam como entidades, como usuários e funções, são persistentes. Execute as lojas de [padrão de repositório](http://deviq.com/repository-pattern/) e estão estreitamente juntamente com o mecanismo de persistência. Gerenciadores de são separados do repositórios, o que significa que você pode substituir o mecanismo de persistência sem alterar o código do aplicativo (com exceção da configuração).

O diagrama a seguir mostra como um aplicativo web interage com os gerentes, enquanto armazena interage com a camada de acesso a dados.

![Aplicativos do ASP.NET Core trabalham com os gerentes (por exemplo, 'UserManager', 'RoleManager'). Gerenciadores de trabalhar com lojas (por exemplo, ' UserStore') que se comunicam com uma fonte de dados usando uma biblioteca como o Entity Framework Core.](identity-custom-storage-providers/_static/identity-architecture-diagram.png)

Para criar um provedor de armazenamento personalizado, crie a fonte de dados, a camada de acesso a dados e as classes de armazenamento que interagem com essa camada de acesso a dados (as caixas verdes e cinza no diagrama acima). Não é necessário personalizar os gerentes ou o código do aplicativo que interage com eles (as caixas azul acima).

Ao criar uma nova instância da `UserManager` ou `RoleManager` fornecer o tipo da classe de usuário e passar uma instância da classe de armazenamento como um argumento. Essa abordagem permite que você conecte suas classes personalizadas ASP.NET Core. 

[Reconfigurar o aplicativo para usar o novo provedor de armazenamento](#reconfigure-app-to-use-new-storage-provider) mostra como instanciar `UserManager` e `RoleManager` com um repositório personalizado.

## <a name="aspnet-core-identity-stores-data-types"></a>Tipos de dados de armazenamentos de identidade do ASP.NET Core

[A identidade do ASP.NET Core](https://github.com/aspnet/identity) tipos de dados são detalhados nas seções a seguir:

### <a name="users"></a>Usuários

Usuários registrados do seu site. O [IdentityUser](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuser) tipo pode ser estendido ou usado como um exemplo para seu próprio tipo personalizado. Você não precisa herdar de um tipo específico para implementar sua própria solução de armazenamento personalizado de identidade.

### <a name="user-claims"></a>Declarações de usuário

Um conjunto de instruções (ou [declarações](https://docs.microsoft.com//dotnet/api/system.security.claims.claim) sobre o usuário que representam a identidade do usuário. Pode habilitar a expressão maior do que a identidade do usuário que pode ser obtido por meio de funções.

### <a name="user-logins"></a>Logons de usuário

Informações sobre o provedor de autenticação externa (como o Facebook ou uma conta da Microsoft) para usar ao fazer logon em um usuário. [Exemplo](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuserlogin)

### <a name="roles"></a>Funções

Grupos de autorização para seu site. Inclui o nome da função Id e a função (como "Admin" ou "Employee"). [Exemplo](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityrole)

## <a name="the-data-access-layer"></a>Camada de acesso a dados

Este tópico pressupõe que você esteja familiarizado com o mecanismo de persistência que você pretende usar e como criar entidades para esse mecanismo. Este tópico fornece detalhes sobre como criar os repositórios ou classes de acesso de dados; Ele fornece algumas sugestões sobre decisões de design ao trabalhar com a identidade do ASP.NET Core.

Você tem uma grande liberdade durante a criação de camada de acesso a dados para um provedor de armazenamento personalizado. Você só precisa criar mecanismos de persistência para os recursos que você pretende usar em seu aplicativo. Por exemplo, se você não estiver usando funções em seu aplicativo, você não precisa criar armazenamento para funções ou associações de função de usuário. A tecnologia e a infraestrutura existente podem exigir uma estrutura que é muito diferente da implementação do padrão de identidade do ASP.NET Core. Na camada de acesso a dados, você deve fornecer a lógica para trabalhar com a estrutura de sua implementação de armazenamento.

A camada de acesso de dados fornece a lógica para salvar os dados de identidade do ASP.NET Core para uma fonte de dados. Camada de acesso a dados para o seu provedor de armazenamento personalizado pode incluir as seguintes classes para armazenar informações de usuário e a função.

### <a name="context-class"></a>Classe de contexto

Encapsula as informações para se conectar ao seu mecanismo de persistência e executar consultas. Várias classes de dados requerem uma instância dessa classe, normalmente é fornecido por meio de injeção de dependência. [Exemplo](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identitydbcontext-1).

### <a name="user-storage"></a>Armazenamento de usuário

Armazena e recupera informações do usuário (como o hash de nome e a senha do usuário). [Exemplo](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="role-storage"></a>Armazenamento de função

Armazena e recupera informações de função (como o nome da função). [Exemplo](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.rolestore-1)

### <a name="userclaims-storage"></a>Armazenamento de UserClaims

Armazena e recupera informações de declaração de usuário (como o tipo de declaração e valor). [Exemplo](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="userlogins-storage"></a>Armazenamento de UserLogins

Armazena e recupera informações de logon do usuário (como um provedor de autenticação externa). [Exemplo](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="userrole-storage"></a>Armazenamento de UserRole

Armazena e recupera a quais funções são atribuídas a quais usuários. [Exemplo](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.userstore-1)

**Dica:** implementar apenas as classes que você pretende usar em seu aplicativo.

Em classes de acesso a dados, forneça o código para executar operações de dados para o mecanismo de persistência. Por exemplo, dentro de um provedor personalizado, você pode ter o código a seguir para criar um novo usuário o *armazenar* classe:

[!code-csharp[Main](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/CustomUserStore.cs?name=createuser&highlight=7)]

A lógica de implementação para criar o usuário está no ``_usersTable.CreateAsync`` método, como mostrado abaixo.

## <a name="customize-the-user-class"></a>Personalizar a classe de usuário

Ao implementar um provedor de armazenamento, criar uma classe de usuário que é equivalente a [ `IdentityUser` classe](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuser).

No mínimo, a classe de usuário deve incluir um `Id` e um `UserName` propriedade.

O `IdentityUser` classe define as propriedades que o ``UserManager`` solicitado de chamadas ao executar operações. O tipo de padrão de `Id` propriedade é uma cadeia de caracteres, mas você pode herdar de `IdentityUser<TKey, TUserClaim, TUserRole, TUserLogin, TUserToken>` e especifique um tipo diferente. A estrutura de espera a implementação de armazenamento para lidar com conversões de tipo de dados.

## <a name="customize-the-user-store"></a>Personalizar o repositório do usuário

Criar um `UserStore` classe que fornece os métodos para todas as operações de dados de usuário. Essa classe é equivalente a [UserStore<TUser> ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.userstore-1) classe. No seu `UserStore` classe, implemente `IUserStore<TUser>` e as interfaces opcionais necessários. Você selecionar quais interfaces opcionais para implementar com base na funcionalidade fornecida no seu aplicativo.

### <a name="optional-interfaces"></a>Interfaces opcionais

- Https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserrolestore-1 IUserRoleStore
- Https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserclaimstore-1 IUserClaimStore
- Https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserpasswordstore-1 IUserPasswordStore
- IUserSecurityStampStore<!-- make these all links and remove / -->
- IUserEmailStore
- IPhoneNumberStore
- IQueryableUserStore
- IUserLoginStore
- IUserTwoFactorStore
- IUserLockoutStore

As interfaces opcionais herdam de `IUserStore`. Você pode ver um usuário de exemplo parcialmente implementada armazenar [aqui](https://github.com/aspnet/Docs/blob/master/aspnetcore/security/authentication/identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/CustomUserStore.cs).

Dentro de `UserStore` classe, que você usar as classes de acesso de dados que você criou para executar operações. Eles são passados usando a injeção de dependência. Por exemplo, no SQL Server com a implementação do Dapper, o `UserStore` classe tem o `CreateAsync` método que usa uma instância de `DapperUsersTable` para inserir um novo registro:

[!code-csharp[Main](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/DapperUsersTable.cs?name=createuser&highlight=7)]

### <a name="interfaces-to-implement-when-customizing-user-store"></a>Interfaces a serem implementadas ao personalizar o repositório do usuário

- **IUserStore**  
 O [IUserStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserstore-1) interface é a única interface, você deve implementar no repositório do usuário. Define métodos para criar, atualizar, excluir e recuperar os usuários.
- **IUserClaimStore**  
 O [IUserClaimStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserclaimstore-1) interface define os métodos a implementar para habilitar as declarações de usuário. Contém métodos para adicionar, remover e recuperando declarações de usuário.
- **IUserLoginStore**  
 O [IUserLoginStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserloginstore-1) define os métodos a implementar para habilitar provedores de autenticação externa. Contém métodos para adicionar, remover e recuperar os logons de usuário e um método para recuperar um usuário com base nas informações de logon.
- **IUserRoleStore**  
 O [IUserRoleStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserrolestore-1) interface define os métodos a implementar para mapear um usuário a uma função. Contém métodos para adicionar, remover e recuperar funções de usuário e um método para verificar se um usuário é atribuído a uma função.
- **IUserPasswordStore**  
 O [IUserPasswordStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserpasswordstore-1) interface define os métodos que você pode implementar para manter as senhas hash. Contém métodos para obter e definir a senha de hash e um método que indica se o usuário tiver definido uma senha.
- **IUserSecurityStampStore**  
 O [IUserSecurityStampStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iusersecuritystampstore-1) interface define os métodos a implementar para usar um carimbo de segurança para que indica se as informações da conta do usuário foi alterado. Esse carimbo é atualizado quando um usuário altera a senha ou adiciona ou remove logons. Contém métodos para obter e definir o carimbo de segurança.
- **IUserTwoFactorStore**  
 O [IUserTwoFactorStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iusertwofactorstore-1) interface define os métodos a implementar para dar suporte à autenticação de dois fatores. Contém métodos para obter e definir se a autenticação de dois fatores é ativada para um usuário.
- **IUserPhoneNumberStore**  
 O [IUserPhoneNumberStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserphonenumberstore-1) interface define os métodos a implementar para armazenar números de telefone do usuário. Contém métodos para obter e definir o número de telefone e se o número de telefone foi confirmado.
- **IUserEmailStore**  
 O [IUserEmailStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuseremailstore-1) interface define os métodos a implementar para armazenar os endereços de email do usuário. Contém métodos para obter e definir o endereço de email e se o email for confirmado.
- **IUserLockoutStore**  
 O [IUserLockoutStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserlockoutstore-1) interface define os métodos a implementar para armazenar informações sobre como bloquear uma conta. Contém métodos para controlar as tentativas de acesso com falha e bloqueios.
- **IQueryableUserStore**  
 O [IQueryableUserStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iqueryableuserstore-1) interface define a implementar membros para fornecer um repositório do usuário que podem ser consultados.

Você pode implementar apenas as interfaces que são necessários em seu aplicativo. Por exemplo:

```csharp
public class UserStore : IUserStore<IdentityUser>,
                         IUserClaimStore<IdentityUser>,
                         IUserLoginStore<IdentityUser>,
                         IUserRoleStore<IdentityUser>,
                         IUserPasswordStore<IdentityUser>,
                         IUserSecurityStampStore<IdentityUser>
{
    // interface implementations not shown
}
```

### <a name="identityuserclaim-identityuserlogin-and-identityuserrole"></a>IdentityUserClaim, IdentityUserLogin e IdentityUserRole

O ``Microsoft.AspNet.Identity.EntityFramework`` namespace contém implementações do [IdentityUserClaim](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuserclaim-1), [IdentityUserLogin](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuserlogin), e [IdentityUserRole](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuserrole-1) classes. Se você estiver usando esses recursos, convém criar suas próprias versões dessas classes e definir as propriedades de seu aplicativo. No entanto, às vezes é mais eficiente para não carregar essas entidades na memória ao executar operações básicas (como adicionar ou remover a declaração do usuário). Em vez disso, as classes de armazenamento de back-end podem executar essas operações diretamente na fonte de dados. Por exemplo, o ``UserStore.GetClaimsAsync`` método pode chamar o ``userClaimTable.FindByUserId(user.Id)`` método para executar uma consulta na tabela diretamente e retornar uma lista de declarações.

## <a name="customize-the-role-class"></a>Personalizar a classe de função

Ao implementar um provedor de armazenamento de função, você pode criar um tipo de função personalizada. Ele não precisa implementar uma interface específica, mas ele deve ter um `Id` e geralmente terá um `Name` propriedade.

A seguir está um exemplo de classe de função:

[!code-csharp[Main](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/ApplicationRole.cs)]

## <a name="customize-the-role-store"></a>Personalizar o repositório de função

Você pode criar um ``RoleStore`` classe que fornece os métodos para todas as operações de dados em funções. Essa classe é equivalente a [RoleStore<TRole> ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.rolestore-1) classe. No `RoleStore` classe, você implementa o ``IRoleStore<TRole>`` e, opcionalmente, o ``IQueryableRoleStore<TRole>`` interface.

- **IRoleStore&lt;TRole&gt;**  
 O [IRoleStore](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.irolestore-1) interface define os métodos para implementar a classe de armazenamento de função. Contém métodos para criar, atualizar, excluir e recuperar funções.
- **Alteração do RoleStore&lt;TRole&gt;**  
 Para personalizar `RoleStore`, crie uma classe que implementa o `IRoleStore` interface. 

## <a name="reconfigure-app-to-use-new-storage-provider"></a>Reconfigurar o aplicativo para usar o novo provedor de armazenamento

Depois de implementar um provedor de armazenamento, você pode configurar seu aplicativo para usá-lo. Se seu aplicativo usado o provedor padrão, substitua-o com o provedor personalizado.

1. Remover o `Microsoft.AspNetCore.EntityFramework.Identity` pacote NuGet.
1. Se o provedor de armazenamento reside em um projeto separado ou um pacote, adicione uma referência a ele.
1. Substitua todas as referências a `Microsoft.AspNetCore.EntityFramework.Identity` com o uso de uma instrução para o namespace de seu provedor de armazenamento.
1. No ``ConfigureServices`` método, altere o `AddIdentity` método para usar seus tipos personalizados. Você pode criar seus próprios métodos de extensão para essa finalidade. Consulte [IdentityServiceCollectionExtensions](https://github.com/aspnet/Identity/blob/rel/1.1.0/src/Microsoft.AspNetCore.Identity/IdentityServiceCollectionExtensions.cs) para obter um exemplo.
1. Se você estiver usando funções, atualize o `RoleManager` para usar o `RoleStore` classe.
1. Atualize a cadeia de caracteres de conexão e as credenciais para a configuração do aplicativo.

Exemplo:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add identity types
    services.AddIdentity<ApplicationUser, ApplicationRole>()
        .AddDefaultTokenProviders();

    // Identity Services
    services.AddTransient<IUserStore<ApplicationUser>, CustomUserStore>();
    services.AddTransient<IRoleStore<ApplicationRole>, CustomRoleStore>();
    string connectionString = Configuration.GetConnectionString("DefaultConnection");
    services.AddTransient<SqlConnection>(e => new SqlConnection(connectionString));
    services.AddTransient<DapperUsersTable>();

    // additional configuration
}
```

## <a name="references"></a>Referências

- [Provedores de armazenamento personalizado para a identidade do ASP.NET](https://docs.microsoft.com/aspnet/identity/overview/extensibility/overview-of-custom-storage-providers-for-aspnet-identity)
- [A identidade do ASP.NET Core](https://github.com/aspnet/identity) -este repositório inclui links para a comunidade mantida provedores de armazenamento.
