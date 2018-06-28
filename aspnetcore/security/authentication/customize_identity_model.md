---
title: Personalização do modelo de identidade
author: ajcvickers
description: Este artigo descreve como personalizar o modelo de dados do Entity Framework Core subjacente para a identidade do ASP.NET Core.
ms.author: avickers
ms.date: 04/12/2018
ms.prod: asp.net-core
uid: security/authentication/customize_identity_model
ms.openlocfilehash: b44b4fd0f24d245b969588a7226ea6aacbe2a722
ms.sourcegitcommit: 7003d27b607e529642ded0400aa48ae692a0e666
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37036886"
---
# <a name="identity-model-customization"></a>Personalização do modelo de identidade

Por [Arthur Vickers](https://github.com/ajcvickers)

Identidade do ASP.NET Core fornece uma estrutura para gerenciar e armazenar as contas de usuário em aplicativos do ASP.NET Core. Identidade é adicionada ao seu projeto quando "Contas de usuário individuais" é selecionada como o mecanismo de autenticação. Por padrão, a identidade faz usar de um Entity Framework (EF) o modelo de dados principal. Este artigo descreve como personalizar o modelo de identidade.

<a name="identity-migrations"></a>

## <a name="identity-and-ef-core-migrations"></a>Identidade e migrações de núcleo EF

Antes de examinar o modelo, é útil entender o funcionamento da identidade com [EF Core migrações](/ef/core/managing-schemas/migrations/) para criar e atualizar um banco de dados. No nível superior, o processo é:

1. Definir ou atualizar um [modelo de dados no código](/ef/core/modeling/).
1. Adicione uma migração para converter esse modelo em alterações que podem ser aplicadas ao banco de dados.
1. Verifique se a migração corretamente representa sua intenção.
1. Aplicam-se a migração para atualizar o banco de dados para ser sincronizado com o modelo.
1. Repita as etapas 1 a 4 para refinar o modelo e manter o banco de dados em sincronia.

As migrações são adicionadas e aplicadas usando:

* O [Package Manager Console](/ef/core/miscellaneous/cli/powershell) (PMC) se você estiver usando o Visual Studio.
* O [comandos dotnet](/ef/core/miscellaneous/cli/dotnet) na linha de comando.
* Selecionando o link de migrações na página de erro quando o aplicativo é executado.

ASP.NET Core tem um manipulador de página de erro de tempo de desenvolvimento que pode ser usado para aplicar as migrações quando o aplicativo é executado. Para aplicativos de produção, geralmente é mais apropriado para gerar scripts SQL de migrações e implantá-las no banco de dados como parte de uma implantação de aplicativo e banco de dados controlada.

Quando um novo aplicativo usando a identidade é criado, as etapas 1 e 2 acima tiveram já foi concluídas. Ou seja, o modelo de dados iniciais já existir e a migração inicial foi adicionada ao projeto. A migração inicial deve ser aplicado ao banco de dados. A migração inicial pode ser feita executando a atualização do banco de dados (PMC), o comando de atualização (.NET Core CLI) de banco de dados de ef dotnet, ou usando a página de erro quando o aplicativo é executado.

As etapas anteriores precisam ser repetido sempre que as alterações são feitas no modelo.

<a name="identity-model"></a>

## <a name="the-identity-model"></a>O modelo de identidade

### <a name="entity-types"></a>Tipos de entidade

O modelo de identidade consiste em sete tipos de entidade:

* `User` -representa o usuário
* `Role` -representa uma função
* `UserClaim` -representa uma declaração que possuem um usuário
* `UserToken` -representa um token de autenticação para um usuário
* `UserLogin` -associa um usuário com um logon
* `RoleClaim` -representa uma declaração que é concedida a todos os usuários dentro de uma função
* `UserRole` -unir a entidade que associa os usuários e funções

### <a name="entity-type-relationships"></a>Relacionamentos de tipo de entidade

Esses tipos de entidade estão relacionados uns aos outros das seguintes maneiras:

* Cada `User` pode ter muitos `UserClaims`
* Cada `User` pode ter muitos `UserLogins`
* Cada `User` pode ter muitos `UserTokens`
* Cada `Role` pode ter muitas associados `RoleClaims`
* Cada `User` pode ter muitas associados `Roles`e cada `Role` pode ser associado a vários usuários
  * Essa é uma relação de muitos-para-muitos, que exige uma tabela de junção no banco de dados. A tabela de junção é representada pelo `UserRole` entidade.

### <a name="default-model-configuration"></a>Configuração de modelo padrão

Identidade define uma variedade de "contexto classes" que herdam de [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) para configurar e usar o modelo. Essa configuração é feita usando o [EF Core código de primeira API fluente](/ef/core/modeling/) no [OnModelCreating](/dotnet/api/microsoft.entityframeworkcore.dbcontext.onmodelcreating#Microsoft_EntityFrameworkCore_DbContext_OnModelCreating_Microsoft_EntityFrameworkCore_ModelBuilder_) método da classe de contexto. A configuração padrão é:

```CSharp
builder.Entity<TUser>(b =>
{
    // Primary key
    b.HasKey(u => u.Id);

    // Indexes for "normalized" username and email, to allow efficient lookups
    b.HasIndex(u => u.NormalizedUserName).HasName("UserNameIndex").IsUnique();
    b.HasIndex(u => u.NormalizedEmail).HasName("EmailIndex");

    // Maps to the AspNetUsers table
    b.ToTable("AspNetUsers");

    // A concurrency token for use with the optimistic concurrency checking
    b.Property(u => u.ConcurrencyStamp).IsConcurrencyToken();

    // Limit the size of columns to use efficient database types
    b.Property(u => u.UserName).HasMaxLength(256);
    b.Property(u => u.NormalizedUserName).HasMaxLength(256);
    b.Property(u => u.Email).HasMaxLength(256);
    b.Property(u => u.NormalizedEmail).HasMaxLength(256);

    // The relationships between User and other entity types
    // Note that these relationships are configured with no navigation properties

    // Each User can have many UserClaims
    b.HasMany<TUserClaim>().WithOne().HasForeignKey(uc => uc.UserId).IsRequired();

    // Each User can have many UserLogins
    b.HasMany<TUserLogin>().WithOne().HasForeignKey(ul => ul.UserId).IsRequired();

    // Each User can have many UserTokens
    b.HasMany<TUserToken>().WithOne().HasForeignKey(ut => ut.UserId).IsRequired();

    // Each User can have many entries in the UserRole join table
    b.HasMany<TUserRole>().WithOne().HasForeignKey(ur => ur.UserId).IsRequired();
});

builder.Entity<TUserClaim>(b =>
{
    // Primary key
    b.HasKey(uc => uc.Id);

    // Maps to the AspNetUserClaims table
    b.ToTable("AspNetUserClaims");
});

builder.Entity<TUserLogin>(b =>
{
    // Composite primary key consisting of the LoginProvider and the key to use
    // with that provider
    b.HasKey(l => new { l.LoginProvider, l.ProviderKey });

    // Limit the size of the composite key columns due to common database restrictions
    b.Property(l => l.LoginProvider).HasMaxLength(128);
    b.Property(l => l.ProviderKey).HasMaxLength(128);

    // Maps to the AspNetUserLogins table
    b.ToTable("AspNetUserLogins");
});

builder.Entity<TUserToken>(b =>
{
    // Composite primary key consisting of the UserId, LoginProvider and Name
    b.HasKey(t => new { t.UserId, t.LoginProvider, t.Name });

    // Limit the size of the composite key columns due to common database restrictions
    b.Property(t => t.LoginProvider).HasMaxLength(maxKeyLength);
    b.Property(t => t.Name).HasMaxLength(maxKeyLength);

    // Maps to the AspNetUserTokens table
    b.ToTable("AspNetUserTokens");
});

builder.Entity<TRole>(b =>
{
    // Primary key
    b.HasKey(r => r.Id);

    // Index for "normalized" role name to allow efficient lookups
    b.HasIndex(r => r.NormalizedName).HasName("RoleNameIndex").IsUnique();

    // Maps to the AspNetRoles table
    b.ToTable("AspNetRoles");

    // A concurrency token for use with the optimistic concurrency checking
    b.Property(r => r.ConcurrencyStamp).IsConcurrencyToken();

    // Limit the size of columns to use efficient database types
    b.Property(u => u.Name).HasMaxLength(256);
    b.Property(u => u.NormalizedName).HasMaxLength(256);

    // The relationships between Role and other entity types
    // Note that these relationships are configured with no navigation properties

    // Each Role can have many entries in the UserRole join table
    b.HasMany<TUserRole>().WithOne().HasForeignKey(ur => ur.RoleId).IsRequired();

    // Each Role can have many associated RoleClaims
    b.HasMany<TRoleClaim>().WithOne().HasForeignKey(rc => rc.RoleId).IsRequired();
});

builder.Entity<TRoleClaim>(b =>
{
    // Primary key
    b.HasKey(rc => rc.Id);

    // Maps to the AspNetRoleClaims table
    b.ToTable("AspNetRoleClaims");
});

builder.Entity<TUserRole>(b =>
{
    // Primary key
    b.HasKey(r => new { r.UserId, r.RoleId });

    // Maps to the AspNetUserRoles table
    b.ToTable("AspNetUserRoles");
});
```

### <a name="model-generic-types"></a>Tipos genéricos do modelo

Identidade define os tipos CLR de padrão para cada um dos tipos de entidade listados acima. Esses tipos são prefixados com "Identity": `IdentityUser`, `IdentityRole`, `IdentityUserClaim`, `IdentityUserToken`, `IdentityUserLogin`, `IdentityRoleClaim`, e `IdentityUserRole`.

Em vez de usar esses tipos diretamente, os tipos podem ser usados como classes base para os tipos do aplicativo. O `DbContext` classes definidas pela identidade são genéricos, de modo que diferentes tipos CLR podem ser usados para um ou mais dos tipos de entidade no modelo. Também permitem desses tipos genéricos para o tipo da chave primária de usuário a ser alterado.

Ao usar a identidade com suporte para funções, uma [IdentityDbContext](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identitydbcontext) classe deve ser usada:

```CSharp
// Uses all the built-in Identity types
// Uses `string` as the key type
public class IdentityDbContext
    : IdentityDbContext<IdentityUser, IdentityRole, string>
{
}

// Uses the built-in Identity types except with a custom User type
// Uses `string` as the key type
public class IdentityDbContext<TUser>
    : IdentityDbContext<TUser, IdentityRole, string>
        where TUser : IdentityUser
{
}

// Uses the built-in Identity types except with custom User and Role types
// The key type is defined by TKey
public class IdentityDbContext<TUser, TRole, TKey>
    : IdentityDbContext<TUser, TRole, TKey, IdentityUserClaim<TKey>, IdentityUserRole<TKey>, IdentityUserLogin<TKey>, IdentityRoleClaim<TKey>, IdentityUserToken<TKey>>
        where TUser : IdentityUser<TKey>
        where TRole : IdentityRole<TKey>
        where TKey : IEquatable<TKey>
{
}

// No built-in Identity types are used; all are specified by generic arguments
// The key type is defined by TKey
public abstract class IdentityDbContext<TUser, TRole, TKey, TUserClaim, TUserRole, TUserLogin, TRoleClaim, TUserToken>
    : IdentityUserContext<TUser, TKey, TUserClaim, TUserLogin, TUserToken>
         where TUser : IdentityUser<TKey>
         where TRole : IdentityRole<TKey>
         where TKey : IEquatable<TKey>
         where TUserClaim : IdentityUserClaim<TKey>
         where TUserRole : IdentityUserRole<TKey>
         where TUserLogin : IdentityUserLogin<TKey>
         where TRoleClaim : IdentityRoleClaim<TKey>
         where TUserToken : IdentityUserToken<TKey>

```

Também é possível usar a identidade sem funções (apenas declarações), caso em que um `IdentityUserContext` classe deve ser usada:


```CSharp
// Uses the built-in non-role Identity types except with a custom User type
// Uses `string` as the key type
public class IdentityUserContext<TUser>
    : IdentityUserContext<TUser, string>
        where TUser : IdentityUser
{
}

// Uses the built-in non-role Identity types except with a custom User type
// The key type is defined by TKey
public class IdentityUserContext<TUser, TKey>
    : IdentityUserContext<TUser, TKey, IdentityUserClaim<TKey>, IdentityUserLogin<TKey>, IdentityUserToken<TKey>>
        where TUser : IdentityUser<TKey>
        where TKey : IEquatable<TKey>
{
}

// No built-in Identity types are used; all are specified by generic arguments, with no roles
// The key type is defined by TKey
public abstract class IdentityUserContext<TUser, TKey, TUserClaim, TUserLogin, TUserToken>
    : DbContext
        where TUser : IdentityUser<TKey>
        where TKey : IEquatable<TKey>
        where TUserClaim : IdentityUserClaim<TKey>
        where TUserLogin : IdentityUserLogin<TKey>
        where TUserToken : IdentityUserToken<TKey>
{
}
```

## <a name="customizing-the-model"></a>Personalização do modelo

O ponto de partida para personalizar o modelo é derivado do tipo de contexto apropriado; Consulte a seção anterior. Esse tipo de contexto normalmente é chamado `ApplicationDbContext` e é criada pelos modelos de ASP.NET Core.

O contexto é usado para configurar o modelo de duas maneiras:
* Fornecendo tipos de entidade e a chave para os parâmetros de tipo genérico.
* Substituindo `OnModelCreating` para modificar o mapeamento desses tipos.

Ao substituir `OnModelCreating`, `base.OnModelCreating` deve ser chamado de pela primeira vez, a substituição de configuração deve ser chamada em seguida. EF Core geralmente tem uma política de última-um-wins para a configuração. Por exemplo, se o `ToTable` para uma entidade tipo é chamada primeiro com o nome de uma tabela e, em seguida, novamente mais tarde com um nome de tabela diferente, em seguida, o nome da tabela na segunda chamada é o que é usado.

### <a name="using-a-custom-user-type"></a>Usando um tipo de usuário personalizado

Para usar um tipo de usuário personalizado, criar o tipo e fazer com que ele herda de `IdentityUser`. É comum para esse tipo de nome `ApplicationUser`. Esse tipo será normalmente têm propriedades adicionais não no tipo base, caso contrário, não haveria nenhum valor em sua criação. Por exemplo:

```CSharp
public class ApplicationUser : IdentityUser
{
    public string CustomTag { get; set; }
}
```

Em seguida, use este tipo como um argumento genérico para o contexto:

```CSharp
public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
}
```

Atualização `ConfigureServices` para usar o novo `ApplicationUser` classe:

```CSharp
services.AddDefaultIdentity<ApplicationUser>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

Não é necessário substituir `OnModelCreating` aqui porque EF Core mapeará a `CustomTag` propriedade por convenção. No entanto, o banco de dados precisa ser atualizada para obter um novo `CustomTag` coluna. Para fazer isso, adicione uma migração e, em seguida, atualize o banco de dados, conforme descrito em [identidade e migrações de núcleo EF](#identity-migrations).

### <a name="changing-the-key-type"></a>Alterar o tipo de chave

Alterando o tipo de uma coluna de chave primária (PK) depois que o banco de dados foi criado é problemático em muitos sistemas de banco de dados. Alterar o PK geralmente envolve descartar e recriar a tabela. Portanto, é recomendável que os tipos de chave seja especificado na migração inicial, de modo que os tipos de chave de destino são criados quando o banco de dados é criado.

Se o banco de dados foi criado, em seguida, `Drop-Database` (PMC) ou `dotnet ef database drop` (.NET Core CLI) pode ser usado para excluí-lo.

Depois de confirmado que o banco de dados não existe, remova a migração inicial com `Remove-Migration` (PMC) ou `dotnet ef migrations remove` (.NET Core CLI).

Atualização de `ApplicationDbContext` para usar uma classe base diferente, especificando o novo tipo de chave para `TKey`. Por exemplo, para usar um `Guid` chave:

```CSharp
public class ApplicationDbContext : IdentityDbContext<IdentityUser<Guid>, IdentityRole<Guid>, Guid>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
}
```

Observe que as classes genéricas `IdentityUser<TKey>` e `IdentityRole<TKey>` também deve ser especificado para usar o novo tipo de chave. `ConfigureServices` deve ser atualizado para usar o usuário genérico:

```CSharp
services.AddDefaultIdentity<IdentityUser<Guid>>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

Se um personalizado `ApplicationUser` está sendo usado, atualizá-la para herdar de `IdentityUser<TKey>`. Por exemplo:

```CSharp
public class ApplicationUser : IdentityUser<Guid>
{
    public string CustomTag { get; set; }
}

public class ApplicationDbContext : IdentityDbContext<ApplicationUser, IdentityRole<Guid>, Guid>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
}
```

```CSharp
services.AddDefaultIdentity<ApplicationUser>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

### <a name="adding-navigation-properties"></a>Adicionando propriedades de navegação

Alterando a configuração de modelo de relações pode ser mais difícil de fazer outras alterações. Deve-se ter cuidado para substituir as relações existentes em vez de criar um novo relações adicionais. Em particular, a relação alterada deve especificar a mesma propriedade de chave estrangeira como a relação existente. Por exemplo, a relação entre `Users` e `UserClaims` por padrão especificado como:

```CSharp
builder.Entity<TUser>(b =>
{
    // Each User can have many UserClaims
    b.HasMany<TUserClaim>()
     .WithOne()
     .HasForeignKey(uc => uc.UserId)
     .IsRequired();
});
```

A chave estrangeira para essa relação é especificada como o `UserClaim.UserId` propriedade. `HasMany` e `WithOne` são chamados sem argumentos para criar a relação sem propriedades de navegação.

Adicionar uma propriedade de navegação para `ApplicationUser` que permitirá associados `UserClaims` seja referenciado do usuário:

```CSharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<IdentityUserClaim<string>> Claims { get; set; }
}
```

Observe que o `TKey` para `IdentityUserClaim<TKey>` é do tipo especificado para a chave primária de usuários – nesse caso `string` como estamos usando os padrões. É **não** o tipo de chave primário para o `UserClaim` tipo de entidade.

Agora que existe a propriedade de navegação deve ser configurado em `OnModelCreating`:

```CSharp
public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        base.OnModelCreating(modelBuilder);

        modelBuilder.Entity<ApplicationUser>(b =>
        {
            // Each User can have many UserClaims
            b.HasMany(e => e.Claims)
                .WithOne()
                .HasForeignKey(uc => uc.UserId)
                .IsRequired();
        });
    }
}
```

Observe que a relação está configurada exatamente como ele era antes, apenas com uma propriedade de navegação especificada na chamada para `HasMany`.

As propriedades de navegação só existem no modelo EF, não o banco de dados. Como a chave estrangeira para a relação não foi alterado, esse tipo de alteração de modelo não requer o banco de dados a ser atualizado. Isso pode ser verificado com a adição de uma migração após fazer a alteração: o `Up` e `Down` métodos estão vazios.

### <a name="adding-all-user-navigation-properties"></a>Adição de todas as propriedades de navegação do usuário

Usando a seção acima como orientação, aqui está um exemplo que configura as propriedades de navegação unidirecional para todas as relações de usuário:

```CSharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<IdentityUserClaim<string>> Claims { get; set; }
    public virtual ICollection<IdentityUserLogin<string>> Logins { get; set; }
    public virtual ICollection<IdentityUserToken<string>> Tokens { get; set; }
    public virtual ICollection<IdentityUserRole<string>> UserRoles { get; set; }
}
```

```CSharp
public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        base.OnModelCreating(modelBuilder);

        modelBuilder.Entity<ApplicationUser>(b =>
        {
            // Each User can have many UserClaims
            b.HasMany(e => e.Claims)
                .WithOne()
                .HasForeignKey(uc => uc.UserId)
                .IsRequired();

            // Each User can have many UserLogins
            b.HasMany(e => e.Logins)
                .WithOne()
                .HasForeignKey(ul => ul.UserId)
                .IsRequired();

            // Each User can have many UserTokens
            b.HasMany(e => e.Tokens)
                .WithOne()
                .HasForeignKey(ut => ut.UserId)
                .IsRequired();

            // Each User can have many entries in the UserRole join table
            b.HasMany(e => e.UserRoles)
                .WithOne()
                .HasForeignKey(ur => ur.UserId)
                .IsRequired();
        });
    }
}
```

### <a name="adding-user-and-role-navigation-properties"></a>Adicionando propriedades de navegação do usuário e a função

Usando a seção acima como orientação, aqui está um exemplo que configura as propriedades de navegação para todas as relações no usuário e a função:

```CSharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<IdentityUserClaim<string>> Claims { get; set; }
    public virtual ICollection<IdentityUserLogin<string>> Logins { get; set; }
    public virtual ICollection<IdentityUserToken<string>> Tokens { get; set; }
    public virtual ICollection<ApplicationUserRole> UserRoles { get; set; }
}

public class ApplicationRole : IdentityRole
{
    public virtual ICollection<ApplicationUserRole> UserRoles { get; set; }
}

public class ApplicationUserRole : IdentityUserRole<string>
{
    public virtual ApplicationUser User { get; set; }
    public virtual ApplicationRole Role { get; set; }
}
```

```CSharp
public class ApplicationDbContext
    : IdentityDbContext<
        ApplicationUser, ApplicationRole, string,
        IdentityUserClaim<string>, ApplicationUserRole, IdentityUserLogin<string>,
        IdentityRoleClaim<string>, IdentityUserToken<string>>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        base.OnModelCreating(modelBuilder);

        modelBuilder.Entity<ApplicationUser>(b =>
        {
            // Each User can have many UserClaims
            b.HasMany(e => e.Claims)
                .WithOne()
                .HasForeignKey(uc => uc.UserId)
                .IsRequired();

            // Each User can have many UserLogins
            b.HasMany(e => e.Logins)
                .WithOne()
                .HasForeignKey(ul => ul.UserId)
                .IsRequired();

            // Each User can have many UserTokens
            b.HasMany(e => e.Tokens)
                .WithOne()
                .HasForeignKey(ut => ut.UserId)
                .IsRequired();

            // Each User can have many entries in the UserRole join table
            b.HasMany(e => e.UserRoles)
                .WithOne(e => e.User)
                .HasForeignKey(ur => ur.UserId)
                .IsRequired();
        });

        modelBuilder.Entity<ApplicationRole>(b =>
        {
            // Each Role can have many entries in the UserRole join table
            b.HasMany(e => e.UserRoles)
                .WithOne(e => e.Role)
                .HasForeignKey(ur => ur.RoleId)
                .IsRequired();
        });

    }
}
```

Notas:

* Este exemplo também inclui o `UserRole` ingressar entidade, que é necessário para navegar a relação muitos-para-muitos dos usuários às funções.
* Lembre-se de alterar os tipos das propriedades de navegação para refletir que `ApplicationXxx` tipos agora estão sendo usados em vez de `IdentityXxx` tipos.
* Lembre-se de usar o `ApplicationXxx` em genérica `ApplicationContext` definição.

### <a name="adding-all-navigation-properties"></a>Adição de todas as propriedades de navegação

Usando a seção acima como orientação, aqui está um exemplo que configura as propriedades de navegação para todas as relações em todos os tipos de entidade:

```CSharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<ApplicationUserClaim> Claims { get; set; }
    public virtual ICollection<ApplicationUserLogin> Logins { get; set; }
    public virtual ICollection<ApplicationUserToken> Tokens { get; set; }
    public virtual ICollection<ApplicationUserRole> UserRoles { get; set; }
}

public class ApplicationRole : IdentityRole
{
    public virtual ICollection<ApplicationUserRole> UserRoles { get; set; }
    public virtual ICollection<ApplicationRoleClaim> RoleClaims { get; set; }
}

public class ApplicationUserRole : IdentityUserRole<string>
{
    public virtual ApplicationUser User { get; set; }
    public virtual ApplicationRole Role { get; set; }
}

public class ApplicationUserClaim : IdentityUserClaim<string>
{
    public virtual ApplicationUser User { get; set; }
}

public class ApplicationUserLogin : IdentityUserLogin<string>
{
    public virtual ApplicationUser User { get; set; }
}

public class ApplicationRoleClaim : IdentityRoleClaim<string>
{
    public virtual ApplicationRole Role { get; set; }
}

public class ApplicationUserToken : IdentityUserToken<string>
{
    public virtual ApplicationUser User { get; set; }
}
```

```CSharp
public class ApplicationDbContext
    : IdentityDbContext<
        ApplicationUser, ApplicationRole, string,
        ApplicationUserClaim, ApplicationUserRole, ApplicationUserLogin,
        ApplicationRoleClaim, ApplicationUserToken>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        base.OnModelCreating(modelBuilder);

        modelBuilder.Entity<ApplicationUser>(b =>
        {
            // Each User can have many UserClaims
            b.HasMany(e => e.Claims)
                .WithOne(e => e.User)
                .HasForeignKey(uc => uc.UserId)
                .IsRequired();

            // Each User can have many UserLogins
            b.HasMany(e => e.Logins)
                .WithOne(e => e.User)
                .HasForeignKey(ul => ul.UserId)
                .IsRequired();

            // Each User can have many UserTokens
            b.HasMany(e => e.Tokens)
                .WithOne(e => e.User)
                .HasForeignKey(ut => ut.UserId)
                .IsRequired();

            // Each User can have many entries in the UserRole join table
            b.HasMany(e => e.UserRoles)
                .WithOne(e => e.User)
                .HasForeignKey(ur => ur.UserId)
                .IsRequired();
        });

        modelBuilder.Entity<ApplicationRole>(b =>
        {
            // Each Role can have many entries in the UserRole join table
            b.HasMany(e => e.UserRoles)
                .WithOne(e => e.Role)
                .HasForeignKey(ur => ur.RoleId)
                .IsRequired();

            // Each Role can have many associated RoleClaims
            b.HasMany(e => e.RoleClaims)
                .WithOne(e => e.Role)
                .HasForeignKey(rc => rc.RoleId)
                .IsRequired();
        });

    }
}
```

### <a name="using-composite-keys"></a>Usando chaves compostas

As seções anteriores demonstraram alterando o tipo de chave usada no modelo de identidade. Alterando o modelo de chave de identidade para usar chaves compostas não é suportado nem recomendado. Usar uma chave composta com identidade envolve alterar como o código do Gerenciador de identidade interage com o modelo, que é uma personalização sem suporte e além do escopo deste documento.

### <a name="changing-tablecolumn-names-and-facets"></a>Alterar nomes de tabela/coluna e facetas

Para alterar os nomes das tabelas e colunas, chame `base.OnModelCreating`e, em seguida, adicione uma configuração para substituir qualquer um dos padrões. Por exemplo, para alterar o nome de todas as tabelas de identidade:

```CSharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.Entity<IdentityUser>(b =>
    {
        b.ToTable("MyUsers");
    });

    modelBuilder.Entity<IdentityUserClaim<string>>(b =>
    {
        b.ToTable("MyUserClaims");
    });

    modelBuilder.Entity<IdentityUserLogin<string>>(b =>
    {
        b.ToTable("MyUserLogins");
    });

    modelBuilder.Entity<IdentityUserToken<string>>(b =>
    {
        b.ToTable("MyUserTokens");
    });

    modelBuilder.Entity<IdentityRole>(b =>
    {
        b.ToTable("MyRoles");
    });

    modelBuilder.Entity<IdentityRoleClaim<string>>(b =>
    {
        b.ToTable("MyRoleClaims");
    });

    modelBuilder.Entity<IdentityUserRole<string>>(b =>
    {
        b.ToTable("MyUserRoles");
    });
}
```

Observe que esses exemplos usam os tipos de identidade padrão. Se você estiver usando um tipo de aplicativo, como `ApplicationUser` , em seguida, configurar esse tipo em vez do tipo padrão.

Aqui está um exemplo que altera alguns nomes de coluna:

```CSharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.Entity<IdentityUser>(b =>
    {
        b.Property(e => e.Email).HasColumnName("EMail");
    });

    modelBuilder.Entity<IdentityUserClaim<string>>(b =>
    {
        b.Property(e => e.ClaimType).HasColumnName("CType");
        b.Property(e => e.ClaimValue).HasColumnName("CValue");
    });
}
```

Alguns tipos de colunas de banco de dados podem ser configurados com determinados _facetas_ como o tamanho máximo permitido. Aqui está um exemplo que define os comprimentos de coluna máximo para várias propriedades de cadeia de caracteres no modelo:

```CSharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.Entity<IdentityUser>(b =>
    {
        b.Property(u => u.UserName).HasMaxLength(128);
        b.Property(u => u.NormalizedUserName).HasMaxLength(128);
        b.Property(u => u.Email).HasMaxLength(128);
        b.Property(u => u.NormalizedEmail).HasMaxLength(128);
    });

    modelBuilder.Entity<IdentityUserToken<string>>(b =>
    {
        b.Property(t => t.LoginProvider).HasMaxLength(128);
        b.Property(t => t.Name).HasMaxLength(128);
    });
}
```

### <a name="mapping-to-a-different-schema"></a>Mapeando para um esquema diferente

Esquemas podem se comportam diferentemente em provedores de banco de dados diferente, mas para SQL Server, o padrão é criar todas as tabelas no esquema "dbo". Isso pode ser alterado para criar as tabelas em um esquema diferente. Por exemplo:

```CSharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.HasDefaultSchema("notdbo");
}
```

### <a name="lazy-loading"></a>Carregamento lento

Nesta seção é adicionado suporte para proxies de carregamento lento no modelo de identidade. Carregamento lento é útil porque ele permite que as propriedades de navegação a ser usado sem primeiro garantir que eles são carregados.

Tipos de entidade podem ser feitos adequados para carregamento lento de diversas maneiras, conforme descrito no [documentação principal do EF](/ef/core/querying/related-data#lazy-loading). Para simplificar, usaremos proxies de carregamento lento, que requer:

* Instalação do [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) pacote.
* Uma chamada para `.UseLazyLoadingProxies()` em `AddDbContext`.
* Tipos de entidade público com propriedades de navegação virtual público.

Aqui está um exemplo de chamada `.UseLazyLoadingProxies()`:

```CSharp
services
    .AddDbContext<ApplicationDbContext>(
        b => b.UseSqlServer(connectionString)
            .UseLazyLoadingProxies())
    .AddDefaultIdentity<ApplicationUser>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

Consulte os exemplos acima para adicionar propriedades de navegação para os tipos de entidade.
