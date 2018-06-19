---
title: Migrar de autenticação de associação do ASP.NET para a identidade do ASP.NET Core 2.0
author: isaac2004
description: Saiba como migrar aplicativos existentes do ASP.NET usando a autenticação de associação para a identidade do ASP.NET Core 2.0.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 04/24/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/proper-to-2x/membership-to-core-identity
ms.openlocfilehash: f0d1099bfda01d036831350e0888ae3830ad3d58
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/08/2018
ms.locfileid: "33851538"
---
# <a name="migrate-from-aspnet-membership-authentication-to-aspnet-core-20-identity"></a><span data-ttu-id="5993f-103">Migrar de autenticação de associação do ASP.NET para a identidade do ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="5993f-103">Migrate from ASP.NET Membership authentication to ASP.NET Core 2.0 Identity</span></span>

<span data-ttu-id="5993f-104">Por [Isaac Levin](https://isaaclevin.com)</span><span class="sxs-lookup"><span data-stu-id="5993f-104">By [Isaac Levin](https://isaaclevin.com)</span></span>

<span data-ttu-id="5993f-105">Este artigo demonstra como migrar o esquema de banco de dados para aplicativos ASP.NET que usam autenticação de associação para a identidade do ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="5993f-105">This article demonstrates migrating the database schema for ASP.NET apps using Membership authentication to ASP.NET Core 2.0 Identity.</span></span>

> [!NOTE]
> <span data-ttu-id="5993f-106">Este documento fornece as etapas necessárias para migrar o esquema de banco de dados para aplicativos baseados na associação do ASP.NET para o esquema de banco de dados usado para a identidade do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5993f-106">This document provides the steps needed to migrate the database schema for ASP.NET Membership-based apps to the database schema used for ASP.NET Core Identity.</span></span> <span data-ttu-id="5993f-107">Para obter mais informações sobre como migrar de autenticação com base em associação ASP.NET para a identidade do ASP.NET, consulte [migrar um aplicativo existente da associação de SQL para a identidade do ASP.NET](/aspnet/identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity).</span><span class="sxs-lookup"><span data-stu-id="5993f-107">For more information about migrating from ASP.NET Membership-based authentication to ASP.NET Identity, see [Migrate an existing app from SQL Membership to ASP.NET Identity](/aspnet/identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity).</span></span> <span data-ttu-id="5993f-108">Para obter mais informações sobre a identidade do ASP.NET Core, consulte [Introdução a identidade do ASP.NET Core](xref:security/authentication/identity).</span><span class="sxs-lookup"><span data-stu-id="5993f-108">For more information about ASP.NET Core Identity, see [Introduction to Identity on ASP.NET Core](xref:security/authentication/identity).</span></span>

## <a name="review-of-membership-schema"></a><span data-ttu-id="5993f-109">Revisão de esquema de associação</span><span class="sxs-lookup"><span data-stu-id="5993f-109">Review of Membership schema</span></span>

<span data-ttu-id="5993f-110">Antes do ASP.NET 2.0, os desenvolvedores foram encarregados de criar todo o processo de autenticação e autorização para seus aplicativos.</span><span class="sxs-lookup"><span data-stu-id="5993f-110">Prior to ASP.NET 2.0, developers were tasked with creating the entire authentication and authorization process for their apps.</span></span> <span data-ttu-id="5993f-111">Com o ASP.NET 2.0, associação foi introduzida, fornecendo uma solução clichê para lidar com a segurança em aplicativos ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="5993f-111">With ASP.NET 2.0, Membership was introduced, providing a boilerplate solution to handling security within ASP.NET apps.</span></span> <span data-ttu-id="5993f-112">Os desenvolvedores agora foram capazes de inicializar um esquema em um banco de dados do SQL Server com o [aspnet_regsql.exe](https://msdn.microsoft.com/library/ms229862.aspx) comando.</span><span class="sxs-lookup"><span data-stu-id="5993f-112">Developers were now able to bootstrap a schema into a SQL Server database with the [aspnet_regsql.exe](https://msdn.microsoft.com/library/ms229862.aspx) command.</span></span> <span data-ttu-id="5993f-113">Depois de executar esse comando, as tabelas a seguir foram criadas no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="5993f-113">After running this command, the following tables were created in the database.</span></span>

  ![Tabelas de associação](identity/_static/membership-tables.png)

<span data-ttu-id="5993f-115">Para migrar aplicativos existentes para a identidade do ASP.NET Core 2.0, os dados nessas tabelas precisam ser migradas para as tabelas usadas pelo novo esquema de identidade.</span><span class="sxs-lookup"><span data-stu-id="5993f-115">To migrate existing apps to ASP.NET Core 2.0 Identity, the data in these tables needs to be migrated to the tables used by the new Identity schema.</span></span>

## <a name="aspnet-core-identity-20-schema"></a><span data-ttu-id="5993f-116">ASP.NET 2.0 de identidade principal esquema</span><span class="sxs-lookup"><span data-stu-id="5993f-116">ASP.NET Core Identity 2.0 schema</span></span>

<span data-ttu-id="5993f-117">ASP.NET Core 2.0 segue o [identidade](/aspnet/identity/index) princípio introduzido no ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="5993f-117">ASP.NET Core 2.0 follows the [Identity](/aspnet/identity/index) principle introduced in ASP.NET 4.5.</span></span> <span data-ttu-id="5993f-118">Embora o princípio for compartilhado, a implementação entre as estruturas é diferente, mesmo entre versões do ASP.NET Core (consulte [migrar autenticação e identidade para o ASP.NET 2.0 de núcleo](xref:migration/1x-to-2x/index)).</span><span class="sxs-lookup"><span data-stu-id="5993f-118">Though the principle is shared, the implementation between the frameworks is different, even between versions of ASP.NET Core (see [Migrate authentication and Identity to ASP.NET Core 2.0](xref:migration/1x-to-2x/index)).</span></span>

<span data-ttu-id="5993f-119">É a maneira mais rápida para exibir o esquema para a identidade do ASP.NET Core 2.0 criar um novo aplicativo ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="5993f-119">The fastest way to view the schema for ASP.NET Core 2.0 Identity is to create a new ASP.NET Core 2.0 app.</span></span> <span data-ttu-id="5993f-120">Siga estas etapas no Visual Studio de 2017:</span><span class="sxs-lookup"><span data-stu-id="5993f-120">Follow these steps in Visual Studio 2017:</span></span>

* <span data-ttu-id="5993f-121">Selecione **Arquivo** > **Novo** > **Projeto**.</span><span class="sxs-lookup"><span data-stu-id="5993f-121">Select **File** > **New** > **Project**.</span></span>
* <span data-ttu-id="5993f-122">Criar um novo **aplicativo Web do ASP.NET Core**e nomeie o projeto *CoreIdentitySample*.</span><span class="sxs-lookup"><span data-stu-id="5993f-122">Create a new **ASP.NET Core Web Application**, and name the project *CoreIdentitySample*.</span></span>
* <span data-ttu-id="5993f-123">Selecione **ASP.NET Core 2.0** na lista suspensa e selecione **Aplicativo Web**.</span><span class="sxs-lookup"><span data-stu-id="5993f-123">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span> <span data-ttu-id="5993f-124">Esse modelo gera um [páginas Razor](xref:mvc/razor-pages/index) aplicativo.</span><span class="sxs-lookup"><span data-stu-id="5993f-124">This template produces a [Razor Pages](xref:mvc/razor-pages/index) app.</span></span> <span data-ttu-id="5993f-125">Antes de clicar em **Okey**, clique em **alterar autenticação**.</span><span class="sxs-lookup"><span data-stu-id="5993f-125">Before clicking **OK**, click **Change Authentication**.</span></span>
* <span data-ttu-id="5993f-126">Escolha **contas de usuário individuais** para os modelos de identidade.</span><span class="sxs-lookup"><span data-stu-id="5993f-126">Choose **Individual User Accounts** for the Identity templates.</span></span> <span data-ttu-id="5993f-127">Por fim, clique em **Okey**, em seguida, **Okey**.</span><span class="sxs-lookup"><span data-stu-id="5993f-127">Finally, click **OK**, then **OK**.</span></span> <span data-ttu-id="5993f-128">Visual Studio cria um projeto usando o modelo de identidade do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5993f-128">Visual Studio creates a project using the ASP.NET Core Identity template.</span></span>

<span data-ttu-id="5993f-129">Identidade de núcleo de ASP.NET 2.0 usa [Entity Framework Core](/ef/core) para interagir com o banco de dados armazenar os dados de autenticação.</span><span class="sxs-lookup"><span data-stu-id="5993f-129">ASP.NET Core 2.0 Identity uses [Entity Framework Core](/ef/core) to interact with the database storing the authentication data.</span></span> <span data-ttu-id="5993f-130">Em ordem para o aplicativo recém-criado trabalhar, deve ser um banco de dados para armazenar esses dados.</span><span class="sxs-lookup"><span data-stu-id="5993f-130">In order for the newly created app to work, there needs to be a database to store this data.</span></span> <span data-ttu-id="5993f-131">Depois de criar um novo aplicativo, a maneira mais rápida para inspecionar o esquema em um ambiente de banco de dados é criar o banco de dados usando migrações do Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="5993f-131">After creating a new app, the fastest way to inspect the schema in a database environment is to create the database using Entity Framework migrations.</span></span> <span data-ttu-id="5993f-132">Esse processo cria um banco de dados, seja localmente ou em outro local, que reflete o esquema.</span><span class="sxs-lookup"><span data-stu-id="5993f-132">This process creates a database, either locally or elsewhere, which mimics that schema.</span></span> <span data-ttu-id="5993f-133">Leia a documentação anterior para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="5993f-133">Review the preceding documentation for more information.</span></span>

<span data-ttu-id="5993f-134">Para criar um banco de dados com o esquema de identidade do ASP.NET Core, execute o `Update-Database` comando no Visual Studio **Package Manager Console** janela (PMC)&mdash;está localizado em **ferramentas**  >  **NuGet Package Manager** > **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="5993f-134">To create a database with the ASP.NET Core Identity schema, run the `Update-Database` command in Visual Studio's **Package Manager Console** (PMC) window&mdash;it's located at **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span> <span data-ttu-id="5993f-135">PMC dá suporte à execução de comandos do Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="5993f-135">PMC supports running Entity Framework commands.</span></span>

<span data-ttu-id="5993f-136">Comandos do Entity Framework usam a cadeia de caracteres de conexão para o banco de dados especificado em *appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="5993f-136">Entity Framework commands use the connection string for the database specified in *appsettings.json*.</span></span> <span data-ttu-id="5993f-137">A seguinte cadeia de conexão tem como alvo um banco de dados em *localhost* chamado *asp net-core identidade*.</span><span class="sxs-lookup"><span data-stu-id="5993f-137">The following connection string targets a database on *localhost* named *asp-net-core-identity*.</span></span> <span data-ttu-id="5993f-138">Nessa configuração, o Entity Framework está configurado para usar o `DefaultConnection` cadeia de caracteres de conexão.</span><span class="sxs-lookup"><span data-stu-id="5993f-138">In this setting, Entity Framework is configured to use the `DefaultConnection` connection string.</span></span>

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=localhost;Database=aspnet-core-identity;Trusted_Connection=True;MultipleActiveResultSets=true"
  }
}
```

<span data-ttu-id="5993f-139">Este comando cria o banco de dados especificado com o esquema e os dados necessários para a inicialização do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="5993f-139">This command builds the database specified with the schema and any data needed for app initialization.</span></span> <span data-ttu-id="5993f-140">A imagem a seguir ilustra a estrutura da tabela que é criada com as etapas anteriores.</span><span class="sxs-lookup"><span data-stu-id="5993f-140">The following image depicts the table structure that's created with the preceding steps.</span></span>

   ![Tabelas de identidade](identity/_static/identity-tables.png)

## <a name="migrate-the-schema"></a><span data-ttu-id="5993f-142">migrar o esquema</span><span class="sxs-lookup"><span data-stu-id="5993f-142">Migrate the schema</span></span>

<span data-ttu-id="5993f-143">Há diferenças sutis nos campos de associação e a identidade do ASP.NET Core e estruturas de tabela.</span><span class="sxs-lookup"><span data-stu-id="5993f-143">There are subtle differences in the table structures and fields for both Membership and ASP.NET Core Identity.</span></span> <span data-ttu-id="5993f-144">O padrão foi alterado substancialmente para autenticação/autorização com aplicativos ASP.NET e ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5993f-144">The pattern has changed substantially for authentication/authorization with ASP.NET and ASP.NET Core apps.</span></span> <span data-ttu-id="5993f-145">Os objetos de chave que ainda são utilizados com identidade são *usuários* e *funções*.</span><span class="sxs-lookup"><span data-stu-id="5993f-145">The key objects that are still used with Identity are *Users* and *Roles*.</span></span> <span data-ttu-id="5993f-146">Aqui estão as tabelas de mapeamento para *usuários*, *funções*, e *UserRoles*.</span><span class="sxs-lookup"><span data-stu-id="5993f-146">Here are mapping tables for *Users*, *Roles*, and *UserRoles*.</span></span>

### <a name="users"></a><span data-ttu-id="5993f-147">Usuários</span><span class="sxs-lookup"><span data-stu-id="5993f-147">Users</span></span>

| <span data-ttu-id="5993f-148">*Identity(AspNetUsers)*</span><span class="sxs-lookup"><span data-stu-id="5993f-148">*Identity(AspNetUsers)*</span></span> |   | <span data-ttu-id="5993f-149">*Membership(aspnet_Users/aspnet_Membership)*</span><span class="sxs-lookup"><span data-stu-id="5993f-149">*Membership(aspnet_Users/aspnet_Membership)*</span></span> ||
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="5993f-150">**Nome do campo**</span><span class="sxs-lookup"><span data-stu-id="5993f-150">**Field Name**</span></span> | <span data-ttu-id="5993f-151">**Tipo**</span><span class="sxs-lookup"><span data-stu-id="5993f-151">**Type**</span></span>  |   <span data-ttu-id="5993f-152">**Nome do campo**</span><span class="sxs-lookup"><span data-stu-id="5993f-152">**Field Name**</span></span> | <span data-ttu-id="5993f-153">**Tipo**</span><span class="sxs-lookup"><span data-stu-id="5993f-153">**Type**</span></span>  |
|`Id` | <span data-ttu-id="5993f-154">cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="5993f-154">string</span></span> | `aspnet_Users.UserId` | <span data-ttu-id="5993f-155">cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="5993f-155">string</span></span>
|`UserName` | <span data-ttu-id="5993f-156">cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="5993f-156">string</span></span> | `aspnet_Users.UserName` | <span data-ttu-id="5993f-157">cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="5993f-157">string</span></span>
|`Email` | <span data-ttu-id="5993f-158">cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="5993f-158">string</span></span> | `aspnet_Membership.Email` | <span data-ttu-id="5993f-159">cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="5993f-159">string</span></span>
|`NormalizedUserName` | <span data-ttu-id="5993f-160">cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="5993f-160">string</span></span> | `aspnet_Users.LoweredUserName` | <span data-ttu-id="5993f-161">cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="5993f-161">string</span></span>
|`NormalizedEmail` | <span data-ttu-id="5993f-162">cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="5993f-162">string</span></span> | `aspnet_Membership.LoweredEmail` | <span data-ttu-id="5993f-163">cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="5993f-163">string</span></span>
|`PhoneNumber` | <span data-ttu-id="5993f-164">cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="5993f-164">string</span></span> | `aspnet_Users.MobileAlias` | <span data-ttu-id="5993f-165">cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="5993f-165">string</span></span>
|`LockoutEnabled` | <span data-ttu-id="5993f-166">bit</span><span class="sxs-lookup"><span data-stu-id="5993f-166">bit</span></span> | `aspnet_Membership.IsLockedOut` | <span data-ttu-id="5993f-167">bit</span><span class="sxs-lookup"><span data-stu-id="5993f-167">bit</span></span>

> [!NOTE]
> <span data-ttu-id="5993f-168">Nem todos os mapeamentos de campo são semelhantes a relações um para um dos membros a identidade do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5993f-168">Not all the field mappings resemble one-to-one relationships from Membership to ASP.NET Core Identity.</span></span> <span data-ttu-id="5993f-169">A tabela anterior usa o esquema de usuário de associação padrão e mapeia para o esquema de identidade do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5993f-169">The preceding table takes the default Membership User schema and maps it to the ASP.NET Core Identity schema.</span></span> <span data-ttu-id="5993f-170">Todos os outros campos personalizados que foram usados para associação precisam ser mapeados manualmente.</span><span class="sxs-lookup"><span data-stu-id="5993f-170">Any other custom fields that were used for Membership need to be mapped manually.</span></span> <span data-ttu-id="5993f-171">Esse mapeamento, não há nenhum mapeamento para senhas, como critérios de senha e a senha salts não migrem entre os dois.</span><span class="sxs-lookup"><span data-stu-id="5993f-171">In this mapping, there's no map for passwords, as both password criteria and password salts don't migrate between the two.</span></span> <span data-ttu-id="5993f-172">**É recomendável deixar a senha como null e pedir que os usuários redefinam suas senhas.**</span><span class="sxs-lookup"><span data-stu-id="5993f-172">**It's recommended to leave the password as null and to ask users to reset their passwords.**</span></span> <span data-ttu-id="5993f-173">No ASP.NET Core Identity, `LockoutEnd` deve ser definida para alguma data no futuro, se o usuário está bloqueado. Isso é mostrado no script de migração.</span><span class="sxs-lookup"><span data-stu-id="5993f-173">In ASP.NET Core Identity, `LockoutEnd` should be set to some date in the future if the user is locked out. This is shown in the migration script.</span></span>

### <a name="roles"></a><span data-ttu-id="5993f-174">Funções</span><span class="sxs-lookup"><span data-stu-id="5993f-174">Roles</span></span>

| <span data-ttu-id="5993f-175">*Identity(AspNetRoles)*</span><span class="sxs-lookup"><span data-stu-id="5993f-175">*Identity(AspNetRoles)*</span></span> |   | <span data-ttu-id="5993f-176">*Membership(aspnet_Roles)*</span><span class="sxs-lookup"><span data-stu-id="5993f-176">*Membership(aspnet_Roles)*</span></span> ||
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="5993f-177">**Nome do campo**</span><span class="sxs-lookup"><span data-stu-id="5993f-177">**Field Name**</span></span> | <span data-ttu-id="5993f-178">**Tipo**</span><span class="sxs-lookup"><span data-stu-id="5993f-178">**Type**</span></span>  |   <span data-ttu-id="5993f-179">**Nome do campo**</span><span class="sxs-lookup"><span data-stu-id="5993f-179">**Field Name**</span></span> | <span data-ttu-id="5993f-180">**Tipo**</span><span class="sxs-lookup"><span data-stu-id="5993f-180">**Type**</span></span>  |
|`Id` | <span data-ttu-id="5993f-181">cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="5993f-181">string</span></span> | `RoleId` | <span data-ttu-id="5993f-182">cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="5993f-182">string</span></span>
|`Name` | <span data-ttu-id="5993f-183">cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="5993f-183">string</span></span> | `RoleName` | <span data-ttu-id="5993f-184">cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="5993f-184">string</span></span>
|`NormalizedName` | <span data-ttu-id="5993f-185">cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="5993f-185">string</span></span> | `LoweredRoleName` | <span data-ttu-id="5993f-186">cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="5993f-186">string</span></span>

### <a name="user-roles"></a><span data-ttu-id="5993f-187">Funções de usuário</span><span class="sxs-lookup"><span data-stu-id="5993f-187">User Roles</span></span>

| <span data-ttu-id="5993f-188">*Identity(AspNetUserRoles)*</span><span class="sxs-lookup"><span data-stu-id="5993f-188">*Identity(AspNetUserRoles)*</span></span> |   | <span data-ttu-id="5993f-189">*Membership(aspnet_UsersInRoles)*</span><span class="sxs-lookup"><span data-stu-id="5993f-189">*Membership(aspnet_UsersInRoles)*</span></span> ||
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="5993f-190">**Nome do campo**</span><span class="sxs-lookup"><span data-stu-id="5993f-190">**Field Name**</span></span> | <span data-ttu-id="5993f-191">**Tipo**</span><span class="sxs-lookup"><span data-stu-id="5993f-191">**Type**</span></span>  |   <span data-ttu-id="5993f-192">**Nome do campo**</span><span class="sxs-lookup"><span data-stu-id="5993f-192">**Field Name**</span></span> | <span data-ttu-id="5993f-193">**Tipo**</span><span class="sxs-lookup"><span data-stu-id="5993f-193">**Type**</span></span>  |
|`RoleId` | <span data-ttu-id="5993f-194">cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="5993f-194">string</span></span> | `RoleId` | <span data-ttu-id="5993f-195">cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="5993f-195">string</span></span>
|`UserId` | <span data-ttu-id="5993f-196">cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="5993f-196">string</span></span> | `UserId` | <span data-ttu-id="5993f-197">cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="5993f-197">string</span></span>

<span data-ttu-id="5993f-198">Referência as tabelas de mapeamento anterior ao criar um script de migração para *usuários* e *funções*.</span><span class="sxs-lookup"><span data-stu-id="5993f-198">Reference the preceding mapping tables when creating a migration script for *Users* and *Roles*.</span></span> <span data-ttu-id="5993f-199">O exemplo a seguir supõe que você tenha dois bancos de dados em um servidor de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="5993f-199">The following example assumes you have two databases on a database server.</span></span> <span data-ttu-id="5993f-200">Um banco de dados contém o esquema de associação ASP.NET existente e os dados.</span><span class="sxs-lookup"><span data-stu-id="5993f-200">One database contains the existing ASP.NET Membership schema and data.</span></span> <span data-ttu-id="5993f-201">Outro banco de dados foi criado usando as etapas descritas anteriormente.</span><span class="sxs-lookup"><span data-stu-id="5993f-201">The other database was created using steps described earlier.</span></span> <span data-ttu-id="5993f-202">Os comentários são incluído embutido para obter mais detalhes.</span><span class="sxs-lookup"><span data-stu-id="5993f-202">Comments are included inline for more details.</span></span>

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
       --Security Stamp is a token used to verify the state of an account and is subject to change at any time. It should be intialized as a new ID.
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

<span data-ttu-id="5993f-203">Após a conclusão do script, o aplicativo ASP.NET Core identidade criado anteriormente é populado com os usuários da associação.</span><span class="sxs-lookup"><span data-stu-id="5993f-203">After completion of this script, the ASP.NET Core Identity app created earlier is populated with Membership users.</span></span> <span data-ttu-id="5993f-204">Os usuários precisam alterar suas senhas antes de efetuar login.</span><span class="sxs-lookup"><span data-stu-id="5993f-204">Users need to change their passwords before logging in.</span></span>

> [!NOTE]
> <span data-ttu-id="5993f-205">Se o sistema de associação teve usuários com nomes de usuário que não correspondem a seu endereço de email, as alterações são necessárias para o aplicativo criado anteriormente para comportá-lo.</span><span class="sxs-lookup"><span data-stu-id="5993f-205">If the Membership system had users with user names that didn't match their email address, changes are required to the app created earlier to accommodate this.</span></span> <span data-ttu-id="5993f-206">O modelo padrão espera `UserName` e `Email` para ser o mesmo.</span><span class="sxs-lookup"><span data-stu-id="5993f-206">The default template expects `UserName` and `Email` to be the same.</span></span> <span data-ttu-id="5993f-207">Em situações em que eles são diferentes, o processo de logon deve ser modificado para usar `UserName` em vez de `Email`.</span><span class="sxs-lookup"><span data-stu-id="5993f-207">For situations in which they're different, the login process needs to be modified to use `UserName` instead of `Email`.</span></span>

<span data-ttu-id="5993f-208">No `PageModel` da página de logon, localizado em *Pages\Account\Login.cshtml.cs*, remova o `[EmailAddress]` de atributo do *Email* propriedade.</span><span class="sxs-lookup"><span data-stu-id="5993f-208">In the `PageModel` of the Login Page, located at *Pages\Account\Login.cshtml.cs*, remove the `[EmailAddress]` attribute from the *Email* property.</span></span> <span data-ttu-id="5993f-209">Renomeie-o para *nome de usuário*.</span><span class="sxs-lookup"><span data-stu-id="5993f-209">Rename it to *UserName*.</span></span> <span data-ttu-id="5993f-210">Isso requer uma alteração onde `EmailAddress` é mencionado no *exibição* e *PageModel*.</span><span class="sxs-lookup"><span data-stu-id="5993f-210">This requires a change wherever `EmailAddress` is mentioned, in the *View* and *PageModel*.</span></span> <span data-ttu-id="5993f-211">O resultado é semelhante ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="5993f-211">The result looks like the following:</span></span>

 ![Logon fixa](identity/_static/fixed-login.png)

## <a name="next-steps"></a><span data-ttu-id="5993f-213">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="5993f-213">Next steps</span></span>

<span data-ttu-id="5993f-214">Neste tutorial, você aprendeu como usuários da associação SQL para a identidade do ASP.NET Core 2.0 da porta.</span><span class="sxs-lookup"><span data-stu-id="5993f-214">In this tutorial, you learned how to port users from SQL membership to ASP.NET Core 2.0 Identity.</span></span> <span data-ttu-id="5993f-215">Para obter mais informações sobre a identidade do ASP.NET Core, consulte [Introdução à identidade](xref:security/authentication/identity).</span><span class="sxs-lookup"><span data-stu-id="5993f-215">For more information regarding ASP.NET Core Identity, see [Introduction to Identity](xref:security/authentication/identity).</span></span>
