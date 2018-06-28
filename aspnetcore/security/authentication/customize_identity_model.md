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
# <a name="identity-model-customization"></a><span data-ttu-id="82bdc-103">Personalização do modelo de identidade</span><span class="sxs-lookup"><span data-stu-id="82bdc-103">Identity model customization</span></span>

<span data-ttu-id="82bdc-104">Por [Arthur Vickers](https://github.com/ajcvickers)</span><span class="sxs-lookup"><span data-stu-id="82bdc-104">By [Arthur Vickers](https://github.com/ajcvickers)</span></span>

<span data-ttu-id="82bdc-105">Identidade do ASP.NET Core fornece uma estrutura para gerenciar e armazenar as contas de usuário em aplicativos do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="82bdc-105">ASP.NET Core Identity provides a framework for managing and storing user accounts in ASP.NET Core applications.</span></span> <span data-ttu-id="82bdc-106">Identidade é adicionada ao seu projeto quando "Contas de usuário individuais" é selecionada como o mecanismo de autenticação.</span><span class="sxs-lookup"><span data-stu-id="82bdc-106">Identity is added to your project when "Individual User Accounts" is selected as the authentication mechanism.</span></span> <span data-ttu-id="82bdc-107">Por padrão, a identidade faz usar de um Entity Framework (EF) o modelo de dados principal.</span><span class="sxs-lookup"><span data-stu-id="82bdc-107">By default, Identity makes use of an Entity Framework (EF) Core data model.</span></span> <span data-ttu-id="82bdc-108">Este artigo descreve como personalizar o modelo de identidade.</span><span class="sxs-lookup"><span data-stu-id="82bdc-108">This article describes how to customize the Identity model.</span></span>

<a name="identity-migrations"></a>

## <a name="identity-and-ef-core-migrations"></a><span data-ttu-id="82bdc-109">Identidade e migrações de núcleo EF</span><span class="sxs-lookup"><span data-stu-id="82bdc-109">Identity and EF Core Migrations</span></span>

<span data-ttu-id="82bdc-110">Antes de examinar o modelo, é útil entender o funcionamento da identidade com [EF Core migrações](/ef/core/managing-schemas/migrations/) para criar e atualizar um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="82bdc-110">Before examining the model, it's useful to understand how Identity works with [EF Core Migrations](/ef/core/managing-schemas/migrations/) to create and update a database.</span></span> <span data-ttu-id="82bdc-111">No nível superior, o processo é:</span><span class="sxs-lookup"><span data-stu-id="82bdc-111">At the top level, the process is:</span></span>

1. <span data-ttu-id="82bdc-112">Definir ou atualizar um [modelo de dados no código](/ef/core/modeling/).</span><span class="sxs-lookup"><span data-stu-id="82bdc-112">Define or update a [data model in code](/ef/core/modeling/).</span></span>
1. <span data-ttu-id="82bdc-113">Adicione uma migração para converter esse modelo em alterações que podem ser aplicadas ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="82bdc-113">Add a migration to translate this model into changes that can be applied to the database.</span></span>
1. <span data-ttu-id="82bdc-114">Verifique se a migração corretamente representa sua intenção.</span><span class="sxs-lookup"><span data-stu-id="82bdc-114">Check that the migration correctly represents your intentions.</span></span>
1. <span data-ttu-id="82bdc-115">Aplicam-se a migração para atualizar o banco de dados para ser sincronizado com o modelo.</span><span class="sxs-lookup"><span data-stu-id="82bdc-115">Apply the migration to update the database to be in sync with the model.</span></span>
1. <span data-ttu-id="82bdc-116">Repita as etapas 1 a 4 para refinar o modelo e manter o banco de dados em sincronia.</span><span class="sxs-lookup"><span data-stu-id="82bdc-116">Repeat steps 1-4 to further refine the model and keep the database in sync.</span></span>

<span data-ttu-id="82bdc-117">As migrações são adicionadas e aplicadas usando:</span><span class="sxs-lookup"><span data-stu-id="82bdc-117">Migrations are added and applied using:</span></span>

* <span data-ttu-id="82bdc-118">O [Package Manager Console](/ef/core/miscellaneous/cli/powershell) (PMC) se você estiver usando o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="82bdc-118">The [Package Manager Console](/ef/core/miscellaneous/cli/powershell) (PMC) if you are using Visual Studio.</span></span>
* <span data-ttu-id="82bdc-119">O [comandos dotnet](/ef/core/miscellaneous/cli/dotnet) na linha de comando.</span><span class="sxs-lookup"><span data-stu-id="82bdc-119">The [dotnet commands](/ef/core/miscellaneous/cli/dotnet) on the command line.</span></span>
* <span data-ttu-id="82bdc-120">Selecionando o link de migrações na página de erro quando o aplicativo é executado.</span><span class="sxs-lookup"><span data-stu-id="82bdc-120">Selecting the migrations link on the error page when the application is run.</span></span>

<span data-ttu-id="82bdc-121">ASP.NET Core tem um manipulador de página de erro de tempo de desenvolvimento que pode ser usado para aplicar as migrações quando o aplicativo é executado.</span><span class="sxs-lookup"><span data-stu-id="82bdc-121">ASP.NET Core has a development-time error page handler that can be used to apply migrations when the application is run.</span></span> <span data-ttu-id="82bdc-122">Para aplicativos de produção, geralmente é mais apropriado para gerar scripts SQL de migrações e implantá-las no banco de dados como parte de uma implantação de aplicativo e banco de dados controlada.</span><span class="sxs-lookup"><span data-stu-id="82bdc-122">For production applications, it is often more appropriate to generate SQL scripts from the migrations and deploy these to the database as part of a controlled application and database deployment.</span></span>

<span data-ttu-id="82bdc-123">Quando um novo aplicativo usando a identidade é criado, as etapas 1 e 2 acima tiveram já foi concluídas.</span><span class="sxs-lookup"><span data-stu-id="82bdc-123">When a new application using Identity is created, steps 1 and 2 above have already been completed.</span></span> <span data-ttu-id="82bdc-124">Ou seja, o modelo de dados iniciais já existir e a migração inicial foi adicionada ao projeto.</span><span class="sxs-lookup"><span data-stu-id="82bdc-124">That is, the initial data model already exists, and the initial migration has been added to the project.</span></span> <span data-ttu-id="82bdc-125">A migração inicial deve ser aplicado ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="82bdc-125">The initial migration still needs to be applied to the database.</span></span> <span data-ttu-id="82bdc-126">A migração inicial pode ser feita executando a atualização do banco de dados (PMC), o comando de atualização (.NET Core CLI) de banco de dados de ef dotnet, ou usando a página de erro quando o aplicativo é executado.</span><span class="sxs-lookup"><span data-stu-id="82bdc-126">The initial migration can be done by running the Update-Database (PMC), the dotnet ef database update (.NET Core CLI) command, or by using the error page when the application is run.</span></span>

<span data-ttu-id="82bdc-127">As etapas anteriores precisam ser repetido sempre que as alterações são feitas no modelo.</span><span class="sxs-lookup"><span data-stu-id="82bdc-127">The preceding steps need to be repeated as changes are made to the model.</span></span>

<a name="identity-model"></a>

## <a name="the-identity-model"></a><span data-ttu-id="82bdc-128">O modelo de identidade</span><span class="sxs-lookup"><span data-stu-id="82bdc-128">The Identity model</span></span>

### <a name="entity-types"></a><span data-ttu-id="82bdc-129">Tipos de entidade</span><span class="sxs-lookup"><span data-stu-id="82bdc-129">Entity types</span></span>

<span data-ttu-id="82bdc-130">O modelo de identidade consiste em sete tipos de entidade:</span><span class="sxs-lookup"><span data-stu-id="82bdc-130">The Identity model consists of seven entity types:</span></span>

* <span data-ttu-id="82bdc-131">`User` -representa o usuário</span><span class="sxs-lookup"><span data-stu-id="82bdc-131">`User` - represents the user</span></span>
* <span data-ttu-id="82bdc-132">`Role` -representa uma função</span><span class="sxs-lookup"><span data-stu-id="82bdc-132">`Role` - represents a role</span></span>
* <span data-ttu-id="82bdc-133">`UserClaim` -representa uma declaração que possuem um usuário</span><span class="sxs-lookup"><span data-stu-id="82bdc-133">`UserClaim` - represents a claim that a user possess</span></span>
* <span data-ttu-id="82bdc-134">`UserToken` -representa um token de autenticação para um usuário</span><span class="sxs-lookup"><span data-stu-id="82bdc-134">`UserToken` - represents an authentication token for a user</span></span>
* <span data-ttu-id="82bdc-135">`UserLogin` -associa um usuário com um logon</span><span class="sxs-lookup"><span data-stu-id="82bdc-135">`UserLogin` - associates a user with a login</span></span>
* <span data-ttu-id="82bdc-136">`RoleClaim` -representa uma declaração que é concedida a todos os usuários dentro de uma função</span><span class="sxs-lookup"><span data-stu-id="82bdc-136">`RoleClaim` - represents a claim that is granted to all users within a role</span></span>
* <span data-ttu-id="82bdc-137">`UserRole` -unir a entidade que associa os usuários e funções</span><span class="sxs-lookup"><span data-stu-id="82bdc-137">`UserRole` - join entity that associates users and roles</span></span>

### <a name="entity-type-relationships"></a><span data-ttu-id="82bdc-138">Relacionamentos de tipo de entidade</span><span class="sxs-lookup"><span data-stu-id="82bdc-138">Entity type relationships</span></span>

<span data-ttu-id="82bdc-139">Esses tipos de entidade estão relacionados uns aos outros das seguintes maneiras:</span><span class="sxs-lookup"><span data-stu-id="82bdc-139">These entity types are related to each other in the following ways:</span></span>

* <span data-ttu-id="82bdc-140">Cada `User` pode ter muitos `UserClaims`</span><span class="sxs-lookup"><span data-stu-id="82bdc-140">Each `User` can have many `UserClaims`</span></span>
* <span data-ttu-id="82bdc-141">Cada `User` pode ter muitos `UserLogins`</span><span class="sxs-lookup"><span data-stu-id="82bdc-141">Each `User` can have many `UserLogins`</span></span>
* <span data-ttu-id="82bdc-142">Cada `User` pode ter muitos `UserTokens`</span><span class="sxs-lookup"><span data-stu-id="82bdc-142">Each `User` can have many `UserTokens`</span></span>
* <span data-ttu-id="82bdc-143">Cada `Role` pode ter muitas associados `RoleClaims`</span><span class="sxs-lookup"><span data-stu-id="82bdc-143">Each `Role` can have many associated `RoleClaims`</span></span>
* <span data-ttu-id="82bdc-144">Cada `User` pode ter muitas associados `Roles`e cada `Role` pode ser associado a vários usuários</span><span class="sxs-lookup"><span data-stu-id="82bdc-144">Each `User` can have many associated `Roles`, and each `Role` can be associated with many Users</span></span>
  * <span data-ttu-id="82bdc-145">Essa é uma relação de muitos-para-muitos, que exige uma tabela de junção no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="82bdc-145">This is a many-to-many relationship, which requires a join table in the database.</span></span> <span data-ttu-id="82bdc-146">A tabela de junção é representada pelo `UserRole` entidade.</span><span class="sxs-lookup"><span data-stu-id="82bdc-146">The join table is represented by the `UserRole` entity.</span></span>

### <a name="default-model-configuration"></a><span data-ttu-id="82bdc-147">Configuração de modelo padrão</span><span class="sxs-lookup"><span data-stu-id="82bdc-147">Default model configuration</span></span>

<span data-ttu-id="82bdc-148">Identidade define uma variedade de "contexto classes" que herdam de [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) para configurar e usar o modelo.</span><span class="sxs-lookup"><span data-stu-id="82bdc-148">Identity defines a variety of "context classes" which inherit from [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) to configure and use the model.</span></span> <span data-ttu-id="82bdc-149">Essa configuração é feita usando o [EF Core código de primeira API fluente](/ef/core/modeling/) no [OnModelCreating](/dotnet/api/microsoft.entityframeworkcore.dbcontext.onmodelcreating#Microsoft_EntityFrameworkCore_DbContext_OnModelCreating_Microsoft_EntityFrameworkCore_ModelBuilder_) método da classe de contexto.</span><span class="sxs-lookup"><span data-stu-id="82bdc-149">This configuration is done using the [EF Core Code First Fluent API](/ef/core/modeling/) in the [OnModelCreating](/dotnet/api/microsoft.entityframeworkcore.dbcontext.onmodelcreating#Microsoft_EntityFrameworkCore_DbContext_OnModelCreating_Microsoft_EntityFrameworkCore_ModelBuilder_) method of the context class.</span></span> <span data-ttu-id="82bdc-150">A configuração padrão é:</span><span class="sxs-lookup"><span data-stu-id="82bdc-150">The default configuration is:</span></span>

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

### <a name="model-generic-types"></a><span data-ttu-id="82bdc-151">Tipos genéricos do modelo</span><span class="sxs-lookup"><span data-stu-id="82bdc-151">Model generic types</span></span>

<span data-ttu-id="82bdc-152">Identidade define os tipos CLR de padrão para cada um dos tipos de entidade listados acima.</span><span class="sxs-lookup"><span data-stu-id="82bdc-152">Identity defines default CLR types for each of the entity types listed above.</span></span> <span data-ttu-id="82bdc-153">Esses tipos são prefixados com "Identity": `IdentityUser`, `IdentityRole`, `IdentityUserClaim`, `IdentityUserToken`, `IdentityUserLogin`, `IdentityRoleClaim`, e `IdentityUserRole`.</span><span class="sxs-lookup"><span data-stu-id="82bdc-153">These types are all prefixed with "Identity": `IdentityUser`, `IdentityRole`, `IdentityUserClaim`, `IdentityUserToken`, `IdentityUserLogin`, `IdentityRoleClaim`, and `IdentityUserRole`.</span></span>

<span data-ttu-id="82bdc-154">Em vez de usar esses tipos diretamente, os tipos podem ser usados como classes base para os tipos do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="82bdc-154">Rather than using these types directly, the types can be used as base classes for the application's own types.</span></span> <span data-ttu-id="82bdc-155">O `DbContext` classes definidas pela identidade são genéricos, de modo que diferentes tipos CLR podem ser usados para um ou mais dos tipos de entidade no modelo.</span><span class="sxs-lookup"><span data-stu-id="82bdc-155">The `DbContext` classes defined by Identity are generic such that different CLR types can be used for one or more of the entity types in the model.</span></span> <span data-ttu-id="82bdc-156">Também permitem desses tipos genéricos para o tipo da chave primária de usuário a ser alterado.</span><span class="sxs-lookup"><span data-stu-id="82bdc-156">These generic types also allow for the type of the User primary key to be changed.</span></span>

<span data-ttu-id="82bdc-157">Ao usar a identidade com suporte para funções, uma [IdentityDbContext](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identitydbcontext) classe deve ser usada:</span><span class="sxs-lookup"><span data-stu-id="82bdc-157">When using Identity with support for roles, an [IdentityDbContext](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identitydbcontext) class should be used:</span></span>

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

<span data-ttu-id="82bdc-158">Também é possível usar a identidade sem funções (apenas declarações), caso em que um `IdentityUserContext` classe deve ser usada:</span><span class="sxs-lookup"><span data-stu-id="82bdc-158">It is also possible to use Identity without roles (only claims), in which case an `IdentityUserContext` class should be used:</span></span>


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

## <a name="customizing-the-model"></a><span data-ttu-id="82bdc-159">Personalização do modelo</span><span class="sxs-lookup"><span data-stu-id="82bdc-159">Customizing the model</span></span>

<span data-ttu-id="82bdc-160">O ponto de partida para personalizar o modelo é derivado do tipo de contexto apropriado; Consulte a seção anterior.</span><span class="sxs-lookup"><span data-stu-id="82bdc-160">The starting point for customizing the model is to derive from the appropriate context type; see the preceding section.</span></span> <span data-ttu-id="82bdc-161">Esse tipo de contexto normalmente é chamado `ApplicationDbContext` e é criada pelos modelos de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="82bdc-161">This context type is customarily called `ApplicationDbContext` and is created by the ASP.NET Core templates.</span></span>

<span data-ttu-id="82bdc-162">O contexto é usado para configurar o modelo de duas maneiras:</span><span class="sxs-lookup"><span data-stu-id="82bdc-162">The context is used to configure the model in two ways:</span></span>
* <span data-ttu-id="82bdc-163">Fornecendo tipos de entidade e a chave para os parâmetros de tipo genérico.</span><span class="sxs-lookup"><span data-stu-id="82bdc-163">By supplying entity and key types for the generic type parameters.</span></span>
* <span data-ttu-id="82bdc-164">Substituindo `OnModelCreating` para modificar o mapeamento desses tipos.</span><span class="sxs-lookup"><span data-stu-id="82bdc-164">By overriding `OnModelCreating` to modify the mapping of these types.</span></span>

<span data-ttu-id="82bdc-165">Ao substituir `OnModelCreating`, `base.OnModelCreating` deve ser chamado de pela primeira vez, a substituição de configuração deve ser chamada em seguida.</span><span class="sxs-lookup"><span data-stu-id="82bdc-165">When overriding `OnModelCreating`, `base.OnModelCreating` should be called first, the overriding configuration should be called next.</span></span> <span data-ttu-id="82bdc-166">EF Core geralmente tem uma política de última-um-wins para a configuração.</span><span class="sxs-lookup"><span data-stu-id="82bdc-166">EF Core generally has a last-one-wins policy for configuration.</span></span> <span data-ttu-id="82bdc-167">Por exemplo, se o `ToTable` para uma entidade tipo é chamada primeiro com o nome de uma tabela e, em seguida, novamente mais tarde com um nome de tabela diferente, em seguida, o nome da tabela na segunda chamada é o que é usado.</span><span class="sxs-lookup"><span data-stu-id="82bdc-167">For example, if the `ToTable` for an entity type is called first with one table name and then again later with a different table name, then the table name in the second call is the one that is used.</span></span>

### <a name="using-a-custom-user-type"></a><span data-ttu-id="82bdc-168">Usando um tipo de usuário personalizado</span><span class="sxs-lookup"><span data-stu-id="82bdc-168">Using a custom User type</span></span>

<span data-ttu-id="82bdc-169">Para usar um tipo de usuário personalizado, criar o tipo e fazer com que ele herda de `IdentityUser`.</span><span class="sxs-lookup"><span data-stu-id="82bdc-169">To use a custom User type, create the type and have it inherit from `IdentityUser`.</span></span> <span data-ttu-id="82bdc-170">É comum para esse tipo de nome `ApplicationUser`.</span><span class="sxs-lookup"><span data-stu-id="82bdc-170">It is customary to name this type `ApplicationUser`.</span></span> <span data-ttu-id="82bdc-171">Esse tipo será normalmente têm propriedades adicionais não no tipo base, caso contrário, não haveria nenhum valor em sua criação.</span><span class="sxs-lookup"><span data-stu-id="82bdc-171">This type will typically have additional properties not in the base type, otherwise there would be no value in creating it.</span></span> <span data-ttu-id="82bdc-172">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="82bdc-172">For example:</span></span>

```CSharp
public class ApplicationUser : IdentityUser
{
    public string CustomTag { get; set; }
}
```

<span data-ttu-id="82bdc-173">Em seguida, use este tipo como um argumento genérico para o contexto:</span><span class="sxs-lookup"><span data-stu-id="82bdc-173">Next use this type as a generic argument for the context:</span></span>

```CSharp
public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
}
```

<span data-ttu-id="82bdc-174">Atualização `ConfigureServices` para usar o novo `ApplicationUser` classe:</span><span class="sxs-lookup"><span data-stu-id="82bdc-174">Update `ConfigureServices` to use the new `ApplicationUser` class:</span></span>

```CSharp
services.AddDefaultIdentity<ApplicationUser>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

<span data-ttu-id="82bdc-175">Não é necessário substituir `OnModelCreating` aqui porque EF Core mapeará a `CustomTag` propriedade por convenção.</span><span class="sxs-lookup"><span data-stu-id="82bdc-175">There is no need to override `OnModelCreating` here because EF Core will map the `CustomTag` property by convention.</span></span> <span data-ttu-id="82bdc-176">No entanto, o banco de dados precisa ser atualizada para obter um novo `CustomTag` coluna.</span><span class="sxs-lookup"><span data-stu-id="82bdc-176">However, the database will need to be updated to get a new `CustomTag` column.</span></span> <span data-ttu-id="82bdc-177">Para fazer isso, adicione uma migração e, em seguida, atualize o banco de dados, conforme descrito em [identidade e migrações de núcleo EF](#identity-migrations).</span><span class="sxs-lookup"><span data-stu-id="82bdc-177">To do this, add a migration, and then update the database as described in  [Identity and EF Core Migrations](#identity-migrations).</span></span>

### <a name="changing-the-key-type"></a><span data-ttu-id="82bdc-178">Alterar o tipo de chave</span><span class="sxs-lookup"><span data-stu-id="82bdc-178">Changing the key type</span></span>

<span data-ttu-id="82bdc-179">Alterando o tipo de uma coluna de chave primária (PK) depois que o banco de dados foi criado é problemático em muitos sistemas de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="82bdc-179">Changing the type of a primary key (PK) column after the database has been created is problematic on many database systems.</span></span> <span data-ttu-id="82bdc-180">Alterar o PK geralmente envolve descartar e recriar a tabela.</span><span class="sxs-lookup"><span data-stu-id="82bdc-180">Changing the PK typically involves dropping and re-creating the table.</span></span> <span data-ttu-id="82bdc-181">Portanto, é recomendável que os tipos de chave seja especificado na migração inicial, de modo que os tipos de chave de destino são criados quando o banco de dados é criado.</span><span class="sxs-lookup"><span data-stu-id="82bdc-181">Therefore, it is recommended that key types be specified in the initial migration such that the target key types are created when the database is created.</span></span>

<span data-ttu-id="82bdc-182">Se o banco de dados foi criado, em seguida, `Drop-Database` (PMC) ou `dotnet ef database drop` (.NET Core CLI) pode ser usado para excluí-lo.</span><span class="sxs-lookup"><span data-stu-id="82bdc-182">If the database has been created, then `Drop-Database` (PMC) or `dotnet ef database drop` (.NET Core CLI) can be used to delete it.</span></span>

<span data-ttu-id="82bdc-183">Depois de confirmado que o banco de dados não existe, remova a migração inicial com `Remove-Migration` (PMC) ou `dotnet ef migrations remove` (.NET Core CLI).</span><span class="sxs-lookup"><span data-stu-id="82bdc-183">Once it is confirmed that the database doesn't exist,  remove the initial migration with `Remove-Migration` (PMC) or `dotnet ef migrations remove` (.NET Core CLI).</span></span>

<span data-ttu-id="82bdc-184">Atualização de `ApplicationDbContext` para usar uma classe base diferente, especificando o novo tipo de chave para `TKey`.</span><span class="sxs-lookup"><span data-stu-id="82bdc-184">Update the `ApplicationDbContext` to use a different base class, specifying the new key type for `TKey`.</span></span> <span data-ttu-id="82bdc-185">Por exemplo, para usar um `Guid` chave:</span><span class="sxs-lookup"><span data-stu-id="82bdc-185">For example, to use a `Guid` key:</span></span>

```CSharp
public class ApplicationDbContext : IdentityDbContext<IdentityUser<Guid>, IdentityRole<Guid>, Guid>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
}
```

<span data-ttu-id="82bdc-186">Observe que as classes genéricas `IdentityUser<TKey>` e `IdentityRole<TKey>` também deve ser especificado para usar o novo tipo de chave.</span><span class="sxs-lookup"><span data-stu-id="82bdc-186">Notice that the generic classes `IdentityUser<TKey>` and `IdentityRole<TKey>` must also be specified to use the new key type.</span></span> <span data-ttu-id="82bdc-187">`ConfigureServices` deve ser atualizado para usar o usuário genérico:</span><span class="sxs-lookup"><span data-stu-id="82bdc-187">`ConfigureServices` must be updated to use the generic user:</span></span>

```CSharp
services.AddDefaultIdentity<IdentityUser<Guid>>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

<span data-ttu-id="82bdc-188">Se um personalizado `ApplicationUser` está sendo usado, atualizá-la para herdar de `IdentityUser<TKey>`.</span><span class="sxs-lookup"><span data-stu-id="82bdc-188">If a custom `ApplicationUser` is being used, update it to inherit from `IdentityUser<TKey>`.</span></span> <span data-ttu-id="82bdc-189">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="82bdc-189">For example:</span></span>

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

### <a name="adding-navigation-properties"></a><span data-ttu-id="82bdc-190">Adicionando propriedades de navegação</span><span class="sxs-lookup"><span data-stu-id="82bdc-190">Adding navigation properties</span></span>

<span data-ttu-id="82bdc-191">Alterando a configuração de modelo de relações pode ser mais difícil de fazer outras alterações.</span><span class="sxs-lookup"><span data-stu-id="82bdc-191">Changing the model configuration for relationships can be a more difficult than making other changes.</span></span> <span data-ttu-id="82bdc-192">Deve-se ter cuidado para substituir as relações existentes em vez de criar um novo relações adicionais.</span><span class="sxs-lookup"><span data-stu-id="82bdc-192">Care must be taken to replace the existing relationships rather than create a new additional relationships.</span></span> <span data-ttu-id="82bdc-193">Em particular, a relação alterada deve especificar a mesma propriedade de chave estrangeira como a relação existente.</span><span class="sxs-lookup"><span data-stu-id="82bdc-193">In particular, the changed relationship must specify the same foreign key property as the existing relationship.</span></span> <span data-ttu-id="82bdc-194">Por exemplo, a relação entre `Users` e `UserClaims` por padrão especificado como:</span><span class="sxs-lookup"><span data-stu-id="82bdc-194">For example, the relationship between `Users` and `UserClaims` is by default specified as:</span></span>

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

<span data-ttu-id="82bdc-195">A chave estrangeira para essa relação é especificada como o `UserClaim.UserId` propriedade.</span><span class="sxs-lookup"><span data-stu-id="82bdc-195">The foreign key for this relationship is specified as the `UserClaim.UserId` property.</span></span> <span data-ttu-id="82bdc-196">`HasMany` e `WithOne` são chamados sem argumentos para criar a relação sem propriedades de navegação.</span><span class="sxs-lookup"><span data-stu-id="82bdc-196">`HasMany` and `WithOne` are called without arguments to create the relationship without navigation properties.</span></span>

<span data-ttu-id="82bdc-197">Adicionar uma propriedade de navegação para `ApplicationUser` que permitirá associados `UserClaims` seja referenciado do usuário:</span><span class="sxs-lookup"><span data-stu-id="82bdc-197">Add a navigation property to `ApplicationUser` that will allow associated `UserClaims` to be referenced from the user:</span></span>

```CSharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<IdentityUserClaim<string>> Claims { get; set; }
}
```

<span data-ttu-id="82bdc-198">Observe que o `TKey` para `IdentityUserClaim<TKey>` é do tipo especificado para a chave primária de usuários – nesse caso `string` como estamos usando os padrões.</span><span class="sxs-lookup"><span data-stu-id="82bdc-198">Note that the `TKey` for `IdentityUserClaim<TKey>` is type specified for the primary key of users--in this case `string` since we're using the defaults.</span></span> <span data-ttu-id="82bdc-199">É **não** o tipo de chave primário para o `UserClaim` tipo de entidade.</span><span class="sxs-lookup"><span data-stu-id="82bdc-199">It is **not** the primary key type for the `UserClaim` entity type.</span></span>

<span data-ttu-id="82bdc-200">Agora que existe a propriedade de navegação deve ser configurado em `OnModelCreating`:</span><span class="sxs-lookup"><span data-stu-id="82bdc-200">Now that the navigation property exists it must be configured in `OnModelCreating`:</span></span>

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

<span data-ttu-id="82bdc-201">Observe que a relação está configurada exatamente como ele era antes, apenas com uma propriedade de navegação especificada na chamada para `HasMany`.</span><span class="sxs-lookup"><span data-stu-id="82bdc-201">Notice that relationship is configured exactly as it was before, only with a navigation property specified in the call to `HasMany`.</span></span>

<span data-ttu-id="82bdc-202">As propriedades de navegação só existem no modelo EF, não o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="82bdc-202">The navigation properties only exist in the EF model, not the database.</span></span> <span data-ttu-id="82bdc-203">Como a chave estrangeira para a relação não foi alterado, esse tipo de alteração de modelo não requer o banco de dados a ser atualizado.</span><span class="sxs-lookup"><span data-stu-id="82bdc-203">Because the foreign key for the relationship has not changed, this kind of model change does not require the database to be updated.</span></span> <span data-ttu-id="82bdc-204">Isso pode ser verificado com a adição de uma migração após fazer a alteração: o `Up` e `Down` métodos estão vazios.</span><span class="sxs-lookup"><span data-stu-id="82bdc-204">This can be checked by adding a migration after making the change: the `Up` and `Down` methods are empty.</span></span>

### <a name="adding-all-user-navigation-properties"></a><span data-ttu-id="82bdc-205">Adição de todas as propriedades de navegação do usuário</span><span class="sxs-lookup"><span data-stu-id="82bdc-205">Adding all User navigation properties</span></span>

<span data-ttu-id="82bdc-206">Usando a seção acima como orientação, aqui está um exemplo que configura as propriedades de navegação unidirecional para todas as relações de usuário:</span><span class="sxs-lookup"><span data-stu-id="82bdc-206">Using the section above as guidance, here is an example that configures unidirectional navigation properties for all relationships on User:</span></span>

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

### <a name="adding-user-and-role-navigation-properties"></a><span data-ttu-id="82bdc-207">Adicionando propriedades de navegação do usuário e a função</span><span class="sxs-lookup"><span data-stu-id="82bdc-207">Adding User and Role navigation properties</span></span>

<span data-ttu-id="82bdc-208">Usando a seção acima como orientação, aqui está um exemplo que configura as propriedades de navegação para todas as relações no usuário e a função:</span><span class="sxs-lookup"><span data-stu-id="82bdc-208">Using the section above as guidance, here is an example that configures navigation properties for all relationships on User and Role:</span></span>

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

<span data-ttu-id="82bdc-209">Notas:</span><span class="sxs-lookup"><span data-stu-id="82bdc-209">Notes:</span></span>

* <span data-ttu-id="82bdc-210">Este exemplo também inclui o `UserRole` ingressar entidade, que é necessário para navegar a relação muitos-para-muitos dos usuários às funções.</span><span class="sxs-lookup"><span data-stu-id="82bdc-210">This example also includes the `UserRole` join entity, which is needed to navigate the many-to-many relationship from Users to Roles.</span></span>
* <span data-ttu-id="82bdc-211">Lembre-se de alterar os tipos das propriedades de navegação para refletir que `ApplicationXxx` tipos agora estão sendo usados em vez de `IdentityXxx` tipos.</span><span class="sxs-lookup"><span data-stu-id="82bdc-211">Remember to change the types of the navigation properties to reflect that `ApplicationXxx` types are now being used instead of `IdentityXxx` types.</span></span>
* <span data-ttu-id="82bdc-212">Lembre-se de usar o `ApplicationXxx` em genérica `ApplicationContext` definição.</span><span class="sxs-lookup"><span data-stu-id="82bdc-212">Remember to use the `ApplicationXxx` in the generic `ApplicationContext` definition.</span></span>

### <a name="adding-all-navigation-properties"></a><span data-ttu-id="82bdc-213">Adição de todas as propriedades de navegação</span><span class="sxs-lookup"><span data-stu-id="82bdc-213">Adding all navigation properties</span></span>

<span data-ttu-id="82bdc-214">Usando a seção acima como orientação, aqui está um exemplo que configura as propriedades de navegação para todas as relações em todos os tipos de entidade:</span><span class="sxs-lookup"><span data-stu-id="82bdc-214">Using the section above as guidance, here is an example that configures navigation properties for all relationships on all entity types:</span></span>

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

### <a name="using-composite-keys"></a><span data-ttu-id="82bdc-215">Usando chaves compostas</span><span class="sxs-lookup"><span data-stu-id="82bdc-215">Using composite keys</span></span>

<span data-ttu-id="82bdc-216">As seções anteriores demonstraram alterando o tipo de chave usada no modelo de identidade.</span><span class="sxs-lookup"><span data-stu-id="82bdc-216">The preceding sections demonstrated changing the type of key used in the Identity model.</span></span> <span data-ttu-id="82bdc-217">Alterando o modelo de chave de identidade para usar chaves compostas não é suportado nem recomendado.</span><span class="sxs-lookup"><span data-stu-id="82bdc-217">Changing the Identity key model to use composite keys is not supported or recommended.</span></span> <span data-ttu-id="82bdc-218">Usar uma chave composta com identidade envolve alterar como o código do Gerenciador de identidade interage com o modelo, que é uma personalização sem suporte e além do escopo deste documento.</span><span class="sxs-lookup"><span data-stu-id="82bdc-218">Using a composite key with Identity would involve changing how the Identity manager code interacts with the model, which is an unsupported customization and beyond the scope of this document.</span></span>

### <a name="changing-tablecolumn-names-and-facets"></a><span data-ttu-id="82bdc-219">Alterar nomes de tabela/coluna e facetas</span><span class="sxs-lookup"><span data-stu-id="82bdc-219">Changing table/column names and facets</span></span>

<span data-ttu-id="82bdc-220">Para alterar os nomes das tabelas e colunas, chame `base.OnModelCreating`e, em seguida, adicione uma configuração para substituir qualquer um dos padrões.</span><span class="sxs-lookup"><span data-stu-id="82bdc-220">To change the names of tables and columns, call `base.OnModelCreating`, and then add configuration to override any of the defaults.</span></span> <span data-ttu-id="82bdc-221">Por exemplo, para alterar o nome de todas as tabelas de identidade:</span><span class="sxs-lookup"><span data-stu-id="82bdc-221">For example, to change the name of all the identity tables:</span></span>

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

<span data-ttu-id="82bdc-222">Observe que esses exemplos usam os tipos de identidade padrão.</span><span class="sxs-lookup"><span data-stu-id="82bdc-222">Note that these examples use the default Identity types.</span></span> <span data-ttu-id="82bdc-223">Se você estiver usando um tipo de aplicativo, como `ApplicationUser` , em seguida, configurar esse tipo em vez do tipo padrão.</span><span class="sxs-lookup"><span data-stu-id="82bdc-223">If you are using an application type, such as `ApplicationUser` then configure that type instead of the default type.</span></span>

<span data-ttu-id="82bdc-224">Aqui está um exemplo que altera alguns nomes de coluna:</span><span class="sxs-lookup"><span data-stu-id="82bdc-224">Here is an example that changes some column names:</span></span>

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

<span data-ttu-id="82bdc-225">Alguns tipos de colunas de banco de dados podem ser configurados com determinados _facetas_ como o tamanho máximo permitido.</span><span class="sxs-lookup"><span data-stu-id="82bdc-225">Some types of database columns can be configured with certain _facets_ such as the maximum string length allowed.</span></span> <span data-ttu-id="82bdc-226">Aqui está um exemplo que define os comprimentos de coluna máximo para várias propriedades de cadeia de caracteres no modelo:</span><span class="sxs-lookup"><span data-stu-id="82bdc-226">Here is an example that sets column max lengths for several string properties in the model:</span></span>

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

### <a name="mapping-to-a-different-schema"></a><span data-ttu-id="82bdc-227">Mapeando para um esquema diferente</span><span class="sxs-lookup"><span data-stu-id="82bdc-227">Mapping to a different schema</span></span>

<span data-ttu-id="82bdc-228">Esquemas podem se comportam diferentemente em provedores de banco de dados diferente, mas para SQL Server, o padrão é criar todas as tabelas no esquema "dbo".</span><span class="sxs-lookup"><span data-stu-id="82bdc-228">Schemas can behave differently in different database providers, but for SQL Server, the default is to create all tables in the "dbo" schema.</span></span> <span data-ttu-id="82bdc-229">Isso pode ser alterado para criar as tabelas em um esquema diferente.</span><span class="sxs-lookup"><span data-stu-id="82bdc-229">This can be changed to instead create the tables in a different schema.</span></span> <span data-ttu-id="82bdc-230">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="82bdc-230">For example:</span></span>

```CSharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.HasDefaultSchema("notdbo");
}
```

### <a name="lazy-loading"></a><span data-ttu-id="82bdc-231">Carregamento lento</span><span class="sxs-lookup"><span data-stu-id="82bdc-231">Lazy loading</span></span>

<span data-ttu-id="82bdc-232">Nesta seção é adicionado suporte para proxies de carregamento lento no modelo de identidade.</span><span class="sxs-lookup"><span data-stu-id="82bdc-232">In this section support for lazy-loading proxies in the Identity model is added.</span></span> <span data-ttu-id="82bdc-233">Carregamento lento é útil porque ele permite que as propriedades de navegação a ser usado sem primeiro garantir que eles são carregados.</span><span class="sxs-lookup"><span data-stu-id="82bdc-233">Lazy-loading is useful since it allows navigation properties to be used without first ensuring they are loaded.</span></span>

<span data-ttu-id="82bdc-234">Tipos de entidade podem ser feitos adequados para carregamento lento de diversas maneiras, conforme descrito no [documentação principal do EF](/ef/core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="82bdc-234">Entity types can be made suitable for lazy-loading in several ways, as described in the [EF Core documentation](/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="82bdc-235">Para simplificar, usaremos proxies de carregamento lento, que requer:</span><span class="sxs-lookup"><span data-stu-id="82bdc-235">For simplicity, we will use lazy-loading proxies, which requires:</span></span>

* <span data-ttu-id="82bdc-236">Instalação do [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) pacote.</span><span class="sxs-lookup"><span data-stu-id="82bdc-236">Installation of the [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) package.</span></span>
* <span data-ttu-id="82bdc-237">Uma chamada para `.UseLazyLoadingProxies()` em `AddDbContext`.</span><span class="sxs-lookup"><span data-stu-id="82bdc-237">A call to `.UseLazyLoadingProxies()` inside `AddDbContext`.</span></span>
* <span data-ttu-id="82bdc-238">Tipos de entidade público com propriedades de navegação virtual público.</span><span class="sxs-lookup"><span data-stu-id="82bdc-238">Public entity types with public virtual navigation properties.</span></span>

<span data-ttu-id="82bdc-239">Aqui está um exemplo de chamada `.UseLazyLoadingProxies()`:</span><span class="sxs-lookup"><span data-stu-id="82bdc-239">Here is an example of calling `.UseLazyLoadingProxies()`:</span></span>

```CSharp
services
    .AddDbContext<ApplicationDbContext>(
        b => b.UseSqlServer(connectionString)
            .UseLazyLoadingProxies())
    .AddDefaultIdentity<ApplicationUser>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

<span data-ttu-id="82bdc-240">Consulte os exemplos acima para adicionar propriedades de navegação para os tipos de entidade.</span><span class="sxs-lookup"><span data-stu-id="82bdc-240">Refer back to the examples above for adding navigation properties to the entity types.</span></span>
