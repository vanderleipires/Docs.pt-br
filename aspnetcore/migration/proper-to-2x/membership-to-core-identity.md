---
title: Migrar de autenticação de associação do ASP.NET para a identidade do ASP.NET Core 2.0
author: isaac2004
description: Saiba como migrar aplicativos existentes do ASP.NET usando a autenticação de associação para a identidade do ASP.NET Core 2.0.
ms.author: scaddie
ms.custom: mvc
ms.date: 04/24/2018
uid: migration/proper-to-2x/membership-to-core-identity
ms.openlocfilehash: 82158ec500151a0bb61fb1da55a53684367d9a4e
ms.sourcegitcommit: 2e054638b69f2b14f6d67d9fa3664999172ee1b2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/14/2018
ms.locfileid: "41824762"
---
# <a name="migrate-from-aspnet-membership-authentication-to-aspnet-core-20-identity"></a>Migrar de autenticação de associação do ASP.NET para a identidade do ASP.NET Core 2.0

Por [Isaac Levin](https://isaaclevin.com)

Este artigo demonstra como migrar o esquema de banco de dados para aplicativos do ASP.NET usando a autenticação de associação para a identidade do ASP.NET Core 2.0.

> [!NOTE]
> Este documento fornece as etapas necessárias para migrar o esquema de banco de dados para aplicativos baseados em associação do ASP.NET para o esquema de banco de dados usado para a identidade do ASP.NET Core. Para obter mais informações sobre como migrar da autenticação baseada em associação do ASP.NET para ASP.NET Identity, consulte [migrar um aplicativo existente da associação do SQL para o ASP.NET Identity](/aspnet/identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity). Para obter mais informações sobre a identidade do ASP.NET Core, consulte [Introdução à identidade no ASP.NET Core](xref:security/authentication/identity).

## <a name="review-of-membership-schema"></a>Revisão do esquema de associação

Antes do ASP.NET 2.0, os desenvolvedores eram encarregados de criar todo o processo de autenticação e autorização para seus aplicativos. Com o ASP.NET 2.0, associação foi introduzida, fornecendo uma solução de texto clichê para lidar com segurança em aplicativos ASP.NET. Os desenvolvedores tiveram acesso agora inicializar um esquema em um banco de dados do SQL Server com o [aspnet_regsql.exe](https://msdn.microsoft.com/library/ms229862.aspx) comando. Depois de executar esse comando, as tabelas a seguir foram criadas no banco de dados.

  ![Tabelas de associação](identity/_static/membership-tables.png)

Para migrar aplicativos existentes para a identidade do ASP.NET Core 2.0, os dados nessas tabelas precisam ser migrados para as tabelas usadas pelo novo esquema de identidade.

## <a name="aspnet-core-identity-20-schema"></a>Esquema do ASP.NET Core 2.0 de identidade

O ASP.NET Core 2.0 segue o [identidade](/aspnet/identity/index) princípio introduzido no ASP.NET 4.5. Embora o princípio é compartilhado, a implementação entre as estruturas é diferente, até mesmo entre as versões do ASP.NET Core (consulte [migrar autenticação e identidade para o ASP.NET Core 2.0](xref:migration/1x-to-2x/index)).

É a maneira mais rápida para exibir o esquema para a identidade do ASP.NET Core 2.0 criar um novo aplicativo ASP.NET Core 2.0. Siga estas etapas no Visual Studio 2017:

* Selecione **Arquivo** > **Novo** > **Projeto**.
* Criar um novo **aplicativo Web ASP.NET Core** e nomeie o projeto *CoreIdentitySample*.
* Selecione **ASP.NET Core 2.0** na lista suspensa e selecione **aplicativo Web**. Esse modelo produz um [páginas do Razor](xref:razor-pages/index) aplicativo. Antes de clicar em **Okey**, clique em **alterar autenticação**.
* Escolher **contas de usuário individuais** para os modelos de identidade. Por fim, clique em **Okey**, em seguida, **Okey**. Visual Studio cria um projeto usando o modelo de identidade do ASP.NET Core.

O ASP.NET Core 2.0 usa de identidade [Entity Framework Core](/ef/core) para interagir com o banco de dados armazenar os dados de autenticação. Em ordem para o aplicativo recém-criado trabalhar, preciso haver um banco de dados para armazenar esses dados. Depois de criar um novo aplicativo, a maneira mais rápida para inspecionar o esquema em um ambiente de banco de dados é criar o banco de dados usando as migrações do Entity Framework. Esse processo cria um banco de dados, localmente ou em outro lugar, que imita o esquema. Examine a documentação anterior para obter mais informações.

Para criar um banco de dados com o esquema de identidade do ASP.NET Core, execute as `Update-Database` comando no Visual Studio **Package Manager Console** janela (PMC)&mdash;ele está localizado no **ferramentas**  >  **Gerenciador de pacotes NuGet** > **Package Manager Console**. PMC dá suporte à execução de comandos de Entity Framework.

Comandos do Entity Framework usam a cadeia de caracteres de conexão para o banco de dados especificado na *appSettings. JSON*. A cadeia de conexão a seguir tem como alvo um banco de dados no *localhost* denominado *asp-net-core-identity*. Nessa configuração, o Entity Framework está configurado para usar o `DefaultConnection` cadeia de caracteres de conexão.

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=localhost;Database=aspnet-core-identity;Trusted_Connection=True;MultipleActiveResultSets=true"
  }
}
```

Este comando cria o banco de dados especificado com o esquema e os dados necessários para a inicialização do aplicativo. A imagem a seguir ilustra a estrutura da tabela que é criada com as etapas anteriores.

   ![Tabelas de identidade](identity/_static/identity-tables.png)

## <a name="migrate-the-schema"></a>Migrar o esquema

Há diferenças sutis nos campos de associação e o ASP.NET Core Identity e estruturas de tabela. O padrão foi alterado significativamente para a autenticação/autorização com aplicativos ASP.NET e ASP.NET Core. Os objetos principais que ainda são utilizados com identidade são *os usuários* e *funções*. Aqui estão as tabelas de mapeamento para *os usuários*, *funções*, e *UserRoles*.

### <a name="users"></a>Usuários

| *Identity(AspNetUsers)* |   | *Membership(aspnet_Users/aspnet_Membership)* ||
| --- | --- | --- | --- | --- | --- |
| **Nome do campo** | **Tipo**  |   **Nome do campo** | **Tipo**  |
|`Id` | cadeia de caracteres | `aspnet_Users.UserId` | cadeia de caracteres
|`UserName` | cadeia de caracteres | `aspnet_Users.UserName` | cadeia de caracteres
|`Email` | cadeia de caracteres | `aspnet_Membership.Email` | cadeia de caracteres
|`NormalizedUserName` | cadeia de caracteres | `aspnet_Users.LoweredUserName` | cadeia de caracteres
|`NormalizedEmail` | cadeia de caracteres | `aspnet_Membership.LoweredEmail` | cadeia de caracteres
|`PhoneNumber` | cadeia de caracteres | `aspnet_Users.MobileAlias` | cadeia de caracteres
|`LockoutEnabled` | bit | `aspnet_Membership.IsLockedOut` | bit

> [!NOTE]
> Nem todos os mapeamentos de campo se parecer com relações um para um dos membros no ASP.NET Core Identity. A tabela anterior usa o esquema de usuário da associação padrão e mapeia-os para o esquema de identidade do ASP.NET Core. Todos os outros campos personalizados que foram usados para associação precisam ser mapeados manualmente. Em que esse mapeamento, não há nenhum mapa para senhas, como critérios de senha e a senha salts não migram entre os dois. **É recomendável deixar a senha como null e pedir que os usuários redefinam suas senhas.** No ASP.NET Core Identity, `LockoutEnd` deverá ser definida para alguma data no futuro, se o usuário está bloqueado. Isso é mostrado no script de migração.

### <a name="roles"></a>Funções

| *Identity(AspNetRoles)* |   | *Membership(aspnet_Roles)* ||
| --- | --- | --- | --- | --- | --- |
| **Nome do campo** | **Tipo**  |   **Nome do campo** | **Tipo**  |
|`Id` | cadeia de caracteres | `RoleId` | cadeia de caracteres
|`Name` | cadeia de caracteres | `RoleName` | cadeia de caracteres
|`NormalizedName` | cadeia de caracteres | `LoweredRoleName` | cadeia de caracteres

### <a name="user-roles"></a>Funções de usuário

| *Identity(AspNetUserRoles)* |   | *Membership(aspnet_UsersInRoles)* ||
| --- | --- | --- | --- | --- | --- |
| **Nome do campo** | **Tipo**  |   **Nome do campo** | **Tipo**  |
|`RoleId` | cadeia de caracteres | `RoleId` | cadeia de caracteres
|`UserId` | cadeia de caracteres | `UserId` | cadeia de caracteres

Fazer referência as tabelas de mapeamento anterior durante a criação de um script de migração para *os usuários* e *funções*. O exemplo a seguir pressupõe que você tenha dois bancos de dados em um servidor de banco de dados. Um banco de dados contém o esquema de associação do ASP.NET existente e os dados. Outro banco de dados foi criado usando as etapas descritas anteriormente. Os comentários são incluídas embutidas para obter mais detalhes.

```sql
-- THIS SCRIPT NEEDS TO RUN FROM THE CONTEXT OF THE MEMBERSHIP DB
BEGIN TRANSACTION MigrateUsersAndRoles
use aspnetdb

-- INSERT USERS
INSERT INTO coreidentity.dbo.aspnetusers
            (id,
             username,
             normalizedusername,
             passwordhash,
             securitystamp,
             emailconfirmed,
             phonenumber,
             phonenumberconfirmed,
             twofactorenabled,
             lockoutend,
             lockoutenabled,
             accessfailedcount,
             email,
             normalizedemail)
SELECT aspnet_users.userid,
       aspnet_users.username,
       aspnet_users.loweredusername,
       --Creates an empty password since passwords don't map between the two schemas
       '',
       --Security Stamp is a token used to verify the state of an account and is subject to change at any time. It should be initialized as a new ID.
       NewID(),
       --EmailConfirmed is set when a new user is created and confirmed via email. Users must have this set during migration to ensure they're able to reset passwords.
       1,
       aspnet_users.mobilealias,
       CASE
         WHEN aspnet_Users.MobileAlias is null THEN 0
         ELSE 1
       END,
       --2-factor Auth likely wasn't setup in Membership for users, so setting as false.
       0,
       CASE
         --Setting lockout date to time in the future (1000 years)
         WHEN aspnet_membership.islockedout = 1 THEN Dateadd(year, 1000,
                                                     Sysutcdatetime())
         ELSE NULL
       END,
       aspnet_membership.islockedout,
       --AccessFailedAccount is used to track failed logins. This is stored in membership in multiple columns. Setting to 0 arbitrarily.
       0,
       aspnet_membership.email,
       aspnet_membership.loweredemail
FROM   aspnet_users
       LEFT OUTER JOIN aspnet_membership
                    ON aspnet_membership.applicationid =
                       aspnet_users.applicationid
                       AND aspnet_users.userid = aspnet_membership.userid
       LEFT OUTER JOIN coreidentity.dbo.aspnetusers
                    ON aspnet_membership.userid = aspnetusers.id
WHERE  aspnetusers.id IS NULL

-- INSERT ROLES
INSERT INTO coreIdentity.dbo.aspnetroles(id,name)
SELECT roleId,rolename
FROM aspnet_roles;

-- INSERT USER ROLES
INSERT INTO coreidentity.dbo.aspnetuserroles(userid,roleid)
SELECT userid,roleid
FROM aspnet_usersinroles;

IF @@ERROR <> 0
  BEGIN
    ROLLBACK TRANSACTION MigrateUsersAndRoles
    RETURN
  END

COMMIT TRANSACTION MigrateUsersAndRoles
```

Após a conclusão do script, o aplicativo ASP.NET Core Identity criado anteriormente é preenchido com os usuários de associação. Os usuários precisam alterar suas senhas antes de fazer logon.

> [!NOTE]
> Se o sistema de associação tivesse os usuários com nomes de usuário que não correspondiam a seu endereço de email, as alterações são necessárias para o aplicativo criado anteriormente para acomodar isso. O modelo padrão espera `UserName` e `Email` sejam iguais. Para situações em que eles são diferentes, o processo de logon deve ser modificado para usar `UserName` em vez de `Email`.

No `PageModel` da página de logon, localizado em *Pages\Account\Login.cshtml.cs*, remova o `[EmailAddress]` de atributos dos *Email* propriedade. Renomeie-o para *nome de usuário*. Isso exige uma alteração sempre que `EmailAddress` mencionada, além de *exibição* e *PageModel*. O resultado é semelhante ao seguinte:

 ![Logon fixa](identity/_static/fixed-login.png)

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você aprendeu a porta que os usuários da associação do SQL para a identidade do ASP.NET Core 2.0. Para obter mais informações sobre a identidade do ASP.NET Core, consulte [Introdução à identidade](xref:security/authentication/identity).
