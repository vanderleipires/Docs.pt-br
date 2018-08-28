---
title: Criar um aplicativo ASP.NET Core com os dados de usuário protegidos por autorização
author: rick-anderson
description: Saiba como criar um aplicativo páginas Razor com dados protegidos por autorização do usuário. Inclui HTTPS, autenticação, segurança, identidade do ASP.NET Core.
ms.author: riande
ms.date: 7/24/2018
uid: security/authorization/secure-data
ms.openlocfilehash: ba59e8d6243965188397c4ba7a130eec42acfb91
ms.sourcegitcommit: 847cc1de5526ff42a7303491e6336c2dbdb45de4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "43055874"
---
::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="6bc3a-104">Ver [esse PDF](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) para a versão do ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-104">See [this PDF](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC version.</span></span> <span data-ttu-id="6bc3a-105">A versão 1.1 do ASP.NET Core deste tutorial está [nesta](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) pasta.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-105">The ASP.NET Core 1.1 version of this tutorial is in [this](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) folder.</span></span> <span data-ttu-id="6bc3a-106">O exemplo do ASP.NET Core 1.1 está em [exemplos](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span><span class="sxs-lookup"><span data-stu-id="6bc3a-106">The 1.1 ASP.NET Core sample is in the [samples](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span></span>
::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="6bc3a-107">Consulte a [esse pdf] (https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_July16_18.pdf)</span><span class="sxs-lookup"><span data-stu-id="6bc3a-107">See the [this pdf] (https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_July16_18.pdf)</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a><span data-ttu-id="6bc3a-108">Criar um aplicativo ASP.NET Core com os dados de usuário protegidos por autorização</span><span class="sxs-lookup"><span data-stu-id="6bc3a-108">Create an ASP.NET Core app with user data protected by authorization</span></span>

<span data-ttu-id="6bc3a-109">Por [Rick Anderson](https://twitter.com/RickAndMSFT) e [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="6bc3a-109">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="6bc3a-110">Este tutorial mostra como criar um aplicativo web do ASP.NET Core com dados de usuário protegidos por autorização.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-110">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="6bc3a-111">Ele exibe uma lista de contatos criada por usuários autenticados (registrados).</span><span class="sxs-lookup"><span data-stu-id="6bc3a-111">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="6bc3a-112">Há três grupos de segurança:</span><span class="sxs-lookup"><span data-stu-id="6bc3a-112">There are three security groups:</span></span>

* <span data-ttu-id="6bc3a-113">**Usuários registrados** podem exibir todos os dados aprovados e podem editar/excluir seus próprios dados.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-113">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="6bc3a-114">**Gerenciadores de** podem aprovar ou rejeitar dados de um contato.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-114">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="6bc3a-115">Apenas os contatos aprovados são visíveis aos usuários.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-115">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="6bc3a-116">**Os administradores** podem rejeitar/aprovar e editar/excluir todos os dados.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-116">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="6bc3a-117">Na imagem a seguir, o usuário Rick (`rick@example.com`) está conectado.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-117">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="6bc3a-118">Rick só pode ver os contatos aprovados e os links **editar**/**excluir**/**criar novo** de seus contatos.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-118">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="6bc3a-119">Somente o último registro criado por Rick exibe os links **editar** e **excluir**.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-119">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="6bc3a-120">Outros usuários não verão o último registro até que um gerente ou administrador altere o status para "Aprovado".</span><span class="sxs-lookup"><span data-stu-id="6bc3a-120">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![imagem descrita anterior](secure-data/_static/rick.png)

<span data-ttu-id="6bc3a-122">Na imagem a seguir, `manager@contoso.com` está conectado e na função de gerenciadores:</span><span class="sxs-lookup"><span data-stu-id="6bc3a-122">In the following image, `manager@contoso.com` is signed in and in the managers role:</span></span>

![imagem descrita anterior](secure-data/_static/manager1.png)

<span data-ttu-id="6bc3a-124">A imagem a seguir mostra a tela de exibição de detalhes de um contato dos gerentes:</span><span class="sxs-lookup"><span data-stu-id="6bc3a-124">The following image shows the managers details view of a contact:</span></span>

![imagem descrita anterior](secure-data/_static/manager.png)

<span data-ttu-id="6bc3a-126">O botões **aprovar** e **rejeitar** são exibidos somente para administradores e gerentes.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-126">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="6bc3a-127">Na imagem a seguir, `admin@contoso.com` está conectado e na função de gerenciadores:</span><span class="sxs-lookup"><span data-stu-id="6bc3a-127">In the following image, `admin@contoso.com` is signed in and in the administrators role:</span></span>

![imagem descrita anterior](secure-data/_static/admin.png)

<span data-ttu-id="6bc3a-129">O administrador tem todos os privilégios.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-129">The administrator has all privileges.</span></span> <span data-ttu-id="6bc3a-130">Ele pode ler/editar/excluir todos os contatos e alterar os status deles.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-130">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="6bc3a-131">O aplicativo foi criado fazendo [scaffolding](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) do seguinte modelo de `Contact`:</span><span class="sxs-lookup"><span data-stu-id="6bc3a-131">The app was created by [scaffolding](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) the following `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet)]

<span data-ttu-id="6bc3a-132">O exemplo contém os seguintes manipuladores de autorização:</span><span class="sxs-lookup"><span data-stu-id="6bc3a-132">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="6bc3a-133">`ContactIsOwnerAuthorizationHandler`: Garante que um usuário só pode editar seus dados.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-133">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="6bc3a-134">`ContactManagerAuthorizationHandler`: Permite que os gerentes aprovem ou rejeitem contatos.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-134">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="6bc3a-135">`ContactAdministratorsAuthorizationHandler`: Permite aos administradores aprovar, rejeitar e editar/excluir contatos.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-135">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6bc3a-136">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="6bc3a-136">Prerequisites</span></span>

<span data-ttu-id="6bc3a-137">Este tutorial é avançado.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-137">This tutorial is advanced.</span></span> <span data-ttu-id="6bc3a-138">Você deve estar familiarizado com:</span><span class="sxs-lookup"><span data-stu-id="6bc3a-138">You should be familiar with:</span></span>

* [<span data-ttu-id="6bc3a-139">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6bc3a-139">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="6bc3a-140">Autenticação</span><span class="sxs-lookup"><span data-stu-id="6bc3a-140">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="6bc3a-141">Confirmação de conta e recuperação de senha</span><span class="sxs-lookup"><span data-stu-id="6bc3a-141">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="6bc3a-142">Autorização</span><span class="sxs-lookup"><span data-stu-id="6bc3a-142">Authorization</span></span>](xref:security/authorization/index)
* [<span data-ttu-id="6bc3a-143">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="6bc3a-143">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<span data-ttu-id="6bc3a-144">O código de download para este tutorial requer a versão prévia do ASP.NET Core 2.2 1 ou posterior.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-144">The download code as for this tutorial requires ASP.NET Core 2.2 preview 1 or later.</span></span> <span data-ttu-id="6bc3a-145">Ver [esse problema de GitHub](https://github.com/aspnet/Identity/issues/1813#issuecomment-394543909) para uma solução alternativa.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-145">See [this GitHub issue](https://github.com/aspnet/Identity/issues/1813#issuecomment-394543909) for a work-around.</span></span>

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="6bc3a-146">O aplicativo inicial e o concluído</span><span class="sxs-lookup"><span data-stu-id="6bc3a-146">The starter and completed app</span></span>

<span data-ttu-id="6bc3a-147">[Baixe](xref:tutorials/index#how-to-download-a-sample) as [concluída](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2) aplicativo.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-147">[Download](xref:tutorials/index#how-to-download-a-sample) the [completed](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2) app.</span></span> <span data-ttu-id="6bc3a-148">[Teste](#test-the-completed-app) o aplicativo concluído para que você se familiarize com seus recursos de segurança.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-148">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="6bc3a-149">O aplicativo inicial</span><span class="sxs-lookup"><span data-stu-id="6bc3a-149">The starter app</span></span>

<span data-ttu-id="6bc3a-150">[Baixe](xref:tutorials/index#how-to-download-a-sample) o aplicativo [inicial](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2).</span><span class="sxs-lookup"><span data-stu-id="6bc3a-150">[Download](xref:tutorials/index#how-to-download-a-sample) the [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2) app.</span></span>

<span data-ttu-id="6bc3a-151">Execute o aplicativo, clique em **ContactManager** e verifique se você pode criar, editar e excluir um contato.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-151">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="6bc3a-152">Proteger os dados de usuário</span><span class="sxs-lookup"><span data-stu-id="6bc3a-152">Secure user data</span></span>

<span data-ttu-id="6bc3a-153">As seções a seguir têm todas as principais etapas para criar um aplicativo com dados de usuários seguros.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-153">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="6bc3a-154">Talvez seja útil para fazer referência ao projeto concluído.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-154">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="6bc3a-155">Vincular os dados de contato ao usuário</span><span class="sxs-lookup"><span data-stu-id="6bc3a-155">Tie the contact data to the user</span></span>

<span data-ttu-id="6bc3a-156">Usar o ASP.NET [identidade](xref:security/authentication/identity) ID de usuário para garantir que os usuários pode editar seus dados, mas não outros dados de usuários.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-156">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="6bc3a-157">Adicione `OwnerID` e `ContactStatus` para o `Contact` modelo:</span><span class="sxs-lookup"><span data-stu-id="6bc3a-157">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

<span data-ttu-id="6bc3a-158">`OwnerID` é a ID do usuário da tabela `AspNetUser` no banco de dados de [identidade](xref:security/authentication/identity).</span><span class="sxs-lookup"><span data-stu-id="6bc3a-158">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="6bc3a-159">O campo `Status` determina se um contato pode ser visto por usuários gerais.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-159">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="6bc3a-160">Criar uma nova migração e atualizar o banco de dados:</span><span class="sxs-lookup"><span data-stu-id="6bc3a-160">Create a new migration and update the database:</span></span>

```console
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-identity"></a><span data-ttu-id="6bc3a-161">Adicionar serviços de função à identidade</span><span class="sxs-lookup"><span data-stu-id="6bc3a-161">Add Role services to Identity</span></span>

<span data-ttu-id="6bc3a-162">Acrescente [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) para adicionar serviços de função:</span><span class="sxs-lookup"><span data-stu-id="6bc3a-162">Append [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) to add Role services:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet2&highlight=12)]

### <a name="require-authenticated-users"></a><span data-ttu-id="6bc3a-163">Exigir usuários autenticados</span><span class="sxs-lookup"><span data-stu-id="6bc3a-163">Require authenticated users</span></span>

<span data-ttu-id="6bc3a-164">Defina a política de autenticação padrão para exigir que os usuários sejam autenticados:</span><span class="sxs-lookup"><span data-stu-id="6bc3a-164">Set the default authentication policy to require users to be authenticated:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet&highlight=17-99)] 

 <span data-ttu-id="6bc3a-165">Você pode recusar a autenticação em nível de método de ação, controlador ou página Razor com o `[AllowAnonymous]` atributo.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-165">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="6bc3a-166">Configurar a política de autenticação padrão para exigir que os usuários sejam autenticados protege recém-adicionado páginas do Razor e controladores.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-166">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="6bc3a-167">Autenticação necessária por padrão é mais segura do que contar com novos controladores e páginas do Razor para incluir o `[Authorize]` atributo.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-167">Having authentication required by default is more secure than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span>

<span data-ttu-id="6bc3a-168">Adicionar [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute)às páginas Índice, Sobre e Contatos para que usuários anônimos possam obter informações sobre o site antes de eles se registrarem.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-168">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index, About, and Contact pages so anonymous users can get information about the site before they register.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Index.cshtml.cs?highlight=1,6)]

### <a name="configure-the-test-account"></a><span data-ttu-id="6bc3a-169">Configurar a conta de teste</span><span class="sxs-lookup"><span data-stu-id="6bc3a-169">Configure the test account</span></span>

<span data-ttu-id="6bc3a-170">A classe `SeedData` cria duas contas: administrador e gerenciador.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-170">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="6bc3a-171">Use a [ferramenta Gerenciador de segredo](xref:security/app-secrets) para definir uma senha para essas contas.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-171">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="6bc3a-172">Defina a senha do diretório do projeto (o diretório que contém *Program.cs*):</span><span class="sxs-lookup"><span data-stu-id="6bc3a-172">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```console
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="6bc3a-173">Se uma senha forte não for especificada, uma exceção é gerada quando `SeedData.Initialize` é chamado.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-173">If a strong password is not specified, an exception is thrown when `SeedData.Initialize` is called.</span></span>

<span data-ttu-id="6bc3a-174">Atualização `Main` para usar a senha de teste:</span><span class="sxs-lookup"><span data-stu-id="6bc3a-174">Update `Main` to use the test password:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="6bc3a-175">Criar as contas de teste e atualizar os contatos</span><span class="sxs-lookup"><span data-stu-id="6bc3a-175">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="6bc3a-176">Atualize o método `Initialize` na classe `SeedData` para criar as contas de teste:</span><span class="sxs-lookup"><span data-stu-id="6bc3a-176">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="6bc3a-177">Adicione a ID de usuário de administrador e `ContactStatus` aos contatos.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-177">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="6bc3a-178">Torne um dos contatos "Enviado" e um "Rejeitado".</span><span class="sxs-lookup"><span data-stu-id="6bc3a-178">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="6bc3a-179">Adicione a ID de usuário e o status para todos os contatos.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-179">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="6bc3a-180">Somente um contato é exibido:</span><span class="sxs-lookup"><span data-stu-id="6bc3a-180">Only one contact is shown:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="6bc3a-181">Criar o proprietário, manager e manipuladores de autorização do administrador</span><span class="sxs-lookup"><span data-stu-id="6bc3a-181">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="6bc3a-182">Criar uma classe `ContactIsOwnerAuthorizationHandler` na pasta *autorização*.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-182">Create a `ContactIsOwnerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="6bc3a-183">O `ContactIsOwnerAuthorizationHandler` verifica que o usuário que atua em um recurso possui o recurso.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-183">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="6bc3a-184">O `ContactIsOwnerAuthorizationHandler` chamadas [contexto. Êxito](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) se o usuário autenticado atual for o proprietário do contato.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-184">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="6bc3a-185">Manipuladores de autorização geralmente:</span><span class="sxs-lookup"><span data-stu-id="6bc3a-185">Authorization handlers generally:</span></span>

* <span data-ttu-id="6bc3a-186">Retornar `context.Succeed` quando os requisitos sejam atendidos.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-186">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="6bc3a-187">Retornar `Task.CompletedTask` quando os requisitos não forem atendidos.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-187">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="6bc3a-188">`Task.CompletedTask` não é sucesso ou falha&mdash;permite que outros manipuladores de autorização executar.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-188">`Task.CompletedTask` is neither success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="6bc3a-189">Se você precisar fazer explicitamente, retornar [contexto. Falha](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span><span class="sxs-lookup"><span data-stu-id="6bc3a-189">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="6bc3a-190">O aplicativo permite que proprietários de contato possam editar/excluir/criar seus dados.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-190">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="6bc3a-191">`ContactIsOwnerAuthorizationHandler` não precisa verificar a operação passada no parâmetro de requisito.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-191">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="6bc3a-192">Criar um manipulador de autorização do Gerenciador</span><span class="sxs-lookup"><span data-stu-id="6bc3a-192">Create a manager authorization handler</span></span>

<span data-ttu-id="6bc3a-193">Criar uma classe `ContactManagerAuthorizationHandler` na pasta *autorização*.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-193">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="6bc3a-194">O `ContactManagerAuthorizationHandler` verifica o usuário atuando no recurso é um gerente.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-194">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="6bc3a-195">Somente gerentes possam aprovar ou rejeitar as alterações de conteúdo (novas ou alteradas).</span><span class="sxs-lookup"><span data-stu-id="6bc3a-195">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="6bc3a-196">Crie um manipulador de autorização do administrador</span><span class="sxs-lookup"><span data-stu-id="6bc3a-196">Create an administrator authorization handler</span></span>

<span data-ttu-id="6bc3a-197">Criar uma classe `ContactAdministratorsAuthorizationHandler` na pasta *autorização*.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-197">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="6bc3a-198">O `ContactAdministratorsAuthorizationHandler` verifica se o usuário atuando no recurso é um administrador.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-198">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="6bc3a-199">Administrador pode fazer todas as operações.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-199">Administrator can do all operations.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="6bc3a-200">O registro dos manipuladores de autorização</span><span class="sxs-lookup"><span data-stu-id="6bc3a-200">Register the authorization handlers</span></span>

<span data-ttu-id="6bc3a-201">Serviços usando o Entity Framework Core devem ser registrados [injeção de dependência](xref:fundamentals/dependency-injection) usando [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span><span class="sxs-lookup"><span data-stu-id="6bc3a-201">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="6bc3a-202">O `ContactIsOwnerAuthorizationHandler` usa o ASP.NET Core [identidade](xref:security/authentication/identity), que se baseia no Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-202">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="6bc3a-203">O registro dos manipuladores com a coleção de serviço para que eles estejam disponíveis para o `ContactsController` por meio [injeção de dependência](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="6bc3a-203">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="6bc3a-204">Adicione o seguinte código ao final da `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="6bc3a-204">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet_defaultPolicy&highlight=27-99)]

<span data-ttu-id="6bc3a-205">`ContactAdministratorsAuthorizationHandler` e `ContactManagerAuthorizationHandler` são adicionados como singletons.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-205">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="6bc3a-206">Eles são singletons porque eles não usam o EF e todas as informações necessárias na `Context` parâmetro do `HandleRequirementAsync` método.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-206">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="6bc3a-207">Suporte à autorização</span><span class="sxs-lookup"><span data-stu-id="6bc3a-207">Support authorization</span></span>

<span data-ttu-id="6bc3a-208">Nesta seção, você atualize as páginas Razor e adicione uma classe de requisitos de operações.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-208">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="6bc3a-209">Examine a classe de requisitos de operações de contato</span><span class="sxs-lookup"><span data-stu-id="6bc3a-209">Review the contact operations requirements class</span></span>

<span data-ttu-id="6bc3a-210">Examine o `ContactOperations` classe.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-210">Review the `ContactOperations` class.</span></span> <span data-ttu-id="6bc3a-211">Essa classe contém os requisitos de aplicativo dá suporte:</span><span class="sxs-lookup"><span data-stu-id="6bc3a-211">This class contains the requirements the app supports:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-razor-pages"></a><span data-ttu-id="6bc3a-212">Criar uma classe base para as páginas do Razor de contatos</span><span class="sxs-lookup"><span data-stu-id="6bc3a-212">Create a base class for the Contacts Razor Pages</span></span>

<span data-ttu-id="6bc3a-213">Crie uma classe base que contém os serviços usados em contatos páginas do Razor.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-213">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="6bc3a-214">A classe base coloca o código de inicialização em um único local:</span><span class="sxs-lookup"><span data-stu-id="6bc3a-214">The base class puts the initialization code in one location:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="6bc3a-215">O código anterior:</span><span class="sxs-lookup"><span data-stu-id="6bc3a-215">The preceding code:</span></span>

* <span data-ttu-id="6bc3a-216">Adiciona o serviço `IAuthorizationService` para acessar os manipuladores de autorização.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-216">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="6bc3a-217">Adiciona o serviço de identidade `UserManager`.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-217">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="6bc3a-218">Adicione a `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-218">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="6bc3a-219">Atualizar o CreateModel</span><span class="sxs-lookup"><span data-stu-id="6bc3a-219">Update the CreateModel</span></span>

<span data-ttu-id="6bc3a-220">Atualize o construtor de criar modelo de página para usar a classe base `DI_BasePageModel`:</span><span class="sxs-lookup"><span data-stu-id="6bc3a-220">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="6bc3a-221">Atualize o método `CreateModel.OnPostAsync` para:</span><span class="sxs-lookup"><span data-stu-id="6bc3a-221">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="6bc3a-222">Adicione a ID de usuário ao modelo `Contact`.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-222">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="6bc3a-223">Chame o manipulador de autorização para verificar se que o usuário tem permissão para criar contatos.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-223">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="6bc3a-224">Atualizar o IndexModel</span><span class="sxs-lookup"><span data-stu-id="6bc3a-224">Update the IndexModel</span></span>

<span data-ttu-id="6bc3a-225">Atualize o método `OnGetAsync` para que apenas os contatos aprovados sejam mostrados aos usuários gerais:</span><span class="sxs-lookup"><span data-stu-id="6bc3a-225">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="6bc3a-226">Atualizar o EditModel</span><span class="sxs-lookup"><span data-stu-id="6bc3a-226">Update the EditModel</span></span>

<span data-ttu-id="6bc3a-227">Adicione um manipulador de autorização para verificar se que o usuário é proprietária do contato.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-227">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="6bc3a-228">Como a autorização de recursos está sendo validada, o `[Authorize]` atributo não é suficiente.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-228">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="6bc3a-229">O aplicativo não tiver acesso ao recurso quando atributos são avaliados.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-229">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="6bc3a-230">Autorização baseada em recursos deve ser imperativa.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-230">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="6bc3a-231">As verificações devem ser executadas depois que o aplicativo tem acesso ao recurso, carregá-los no modelo de página ou carregá-los dentro do manipulador em si.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-231">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="6bc3a-232">Com frequência, você acessa o recurso, passando a chave de recurso.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-232">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="6bc3a-233">Atualizar o DeleteModel</span><span class="sxs-lookup"><span data-stu-id="6bc3a-233">Update the DeleteModel</span></span>

<span data-ttu-id="6bc3a-234">Atualize o modelo de página de exclusão para usar o manipulador de autorização para verificar se que o usuário tem permissão de exclusão no contato.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-234">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="6bc3a-235">Injetar o serviço de autorização em exibições</span><span class="sxs-lookup"><span data-stu-id="6bc3a-235">Inject the authorization service into the views</span></span>

<span data-ttu-id="6bc3a-236">Atualmente, mostra a interface do usuário a edita e excluir links para os contatos, que o usuário não é possível modificar.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-236">Currently, the UI shows edit and delete links for contacts the user can't modify.</span></span>

<span data-ttu-id="6bc3a-237">Injetar o serviço de autorização no arquivo *Views/_ViewImports.cshtml* para que ele esteja disponível para todos os modos de exibição:</span><span class="sxs-lookup"><span data-stu-id="6bc3a-237">Inject the authorization service in the *Views/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/_ViewImports.cshtml?highlight=6-99)]

<span data-ttu-id="6bc3a-238">A marcação anterior adiciona várias `using` instruções.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-238">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="6bc3a-239">Atualizar o **editar** e **excluir** vincula na *Pages/Contacts/Index.cshtml* para que eles sejam renderizados somente para usuários com as permissões apropriadas:</span><span class="sxs-lookup"><span data-stu-id="6bc3a-239">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> <span data-ttu-id="6bc3a-240">Ocultar links de usuários que não tem permissão para alterar os dados não proteger o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-240">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="6bc3a-241">Ocultar links torna o aplicativo mais fácil de usar, exibindo links só é válidas.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-241">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="6bc3a-242">Os usuários podem hack gerados URLs para invocar editar e excluir operações em dados que não possuem.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-242">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="6bc3a-243">A página do Razor ou controlador deve impor verificações de acesso para proteger os dados.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-243">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="6bc3a-244">Detalhes da atualização</span><span class="sxs-lookup"><span data-stu-id="6bc3a-244">Update Details</span></span>

<span data-ttu-id="6bc3a-245">Atualize a exibição de detalhes para que os gerentes possam aprovar ou rejeitar contatos:</span><span class="sxs-lookup"><span data-stu-id="6bc3a-245">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml?name=snippet)]

<span data-ttu-id="6bc3a-246">Atualize o modelo de página de detalhes:</span><span class="sxs-lookup"><span data-stu-id="6bc3a-246">Update the details page model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="test-the-completed-app"></a><span data-ttu-id="6bc3a-247">Testar o aplicativo concluído</span><span class="sxs-lookup"><span data-stu-id="6bc3a-247">Test the completed app</span></span>

<span data-ttu-id="6bc3a-248">Se o aplicativo tem contatos:</span><span class="sxs-lookup"><span data-stu-id="6bc3a-248">If the app has contacts:</span></span>

* <span data-ttu-id="6bc3a-249">Excluir todos os registros da tabela `Contact` .</span><span class="sxs-lookup"><span data-stu-id="6bc3a-249">Delete all the records in the `Contact` table.</span></span>
* <span data-ttu-id="6bc3a-250">Reinicie o aplicativo para propagar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-250">Restart the app to seed the database.</span></span>

<span data-ttu-id="6bc3a-251">Registre um usuário para os contatos de navegação.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-251">Register a user for browsing the contacts.</span></span>

<span data-ttu-id="6bc3a-252">Uma maneira fácil de testar o aplicativo concluído é iniciar três diferentes navegadores (ou versões de janela anônima/InPrivate).</span><span class="sxs-lookup"><span data-stu-id="6bc3a-252">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate versions).</span></span> <span data-ttu-id="6bc3a-253">Em um navegador, registre um novo usuário (por exemplo, `test@example.com`).</span><span class="sxs-lookup"><span data-stu-id="6bc3a-253">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="6bc3a-254">Entrar para cada navegador com um usuário diferente.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-254">Sign in to each browser with a different user.</span></span> <span data-ttu-id="6bc3a-255">Verifique se as seguintes operações:</span><span class="sxs-lookup"><span data-stu-id="6bc3a-255">Verify the following operations:</span></span>

* <span data-ttu-id="6bc3a-256">Usuários registrados podem exibir todos os dados de contato aprovados.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-256">Registered users can view all the approved contact data.</span></span>
* <span data-ttu-id="6bc3a-257">Os usuários registrados podem editar/excluir seus próprios dados.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-257">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="6bc3a-258">Os gerentes podem aprovar ou rejeitar contatos.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-258">Managers can approve or reject contact data.</span></span> <span data-ttu-id="6bc3a-259">A tela `Details` mostra os botões **aprovar** e **rejeitar**.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-259">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="6bc3a-260">Os administradores podem Aprovar/rejeitar e editar/excluir todos os dados.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-260">Administrators can approve/reject and edit/delete any data.</span></span>

| <span data-ttu-id="6bc3a-261">User</span><span class="sxs-lookup"><span data-stu-id="6bc3a-261">User</span></span>| <span data-ttu-id="6bc3a-262">Opções</span><span class="sxs-lookup"><span data-stu-id="6bc3a-262">Options</span></span> |
| ------------ | ---------|
| test@example.com | <span data-ttu-id="6bc3a-263">Pode editar/excluir possuem dados</span><span class="sxs-lookup"><span data-stu-id="6bc3a-263">Can edit/delete own data</span></span> |
| manager@contoso.com | <span data-ttu-id="6bc3a-264">Podem Aprovar/rejeitar e Editar/Excluir proprietário dados</span><span class="sxs-lookup"><span data-stu-id="6bc3a-264">Can approve/reject and edit/delete own data</span></span> |
| admin@contoso.com | <span data-ttu-id="6bc3a-265">Pode editar/excluir e Aprovar/rejeitar todos os dados</span><span class="sxs-lookup"><span data-stu-id="6bc3a-265">Can edit/delete and approve/reject all data</span></span>|

<span data-ttu-id="6bc3a-266">Crie um contato no navegador do administrador.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-266">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="6bc3a-267">Copie a URL para excluir e editar a partir do contato do administrador.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-267">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="6bc3a-268">Cole esses links no navegador do usuário de teste para verificar se que o usuário de teste não é possível executar essas operações.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-268">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="6bc3a-269">Criar o aplicativo inicial</span><span class="sxs-lookup"><span data-stu-id="6bc3a-269">Create the starter app</span></span>

* <span data-ttu-id="6bc3a-270">Criar um aplicativo páginas Razor denominado "ContactManager"</span><span class="sxs-lookup"><span data-stu-id="6bc3a-270">Create a Razor Pages app named "ContactManager"</span></span>
   * <span data-ttu-id="6bc3a-271">Criar o aplicativo com **contas de usuário individuais**.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-271">Create the app with **Individual User Accounts**.</span></span>
   * <span data-ttu-id="6bc3a-272">O nome "ContactManager" para o namespace coincida com o namespace usado no exemplo.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-272">Name it "ContactManager" so the namespace matches the namespace used in the sample.</span></span>
   * <span data-ttu-id="6bc3a-273">`-uld` Especifica o LocalDB em vez do SQLite</span><span class="sxs-lookup"><span data-stu-id="6bc3a-273">`-uld` specifies LocalDB instead of SQLite</span></span>

  ```console
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* <span data-ttu-id="6bc3a-274">Adicione *Models\Contact.cs*:</span><span class="sxs-lookup"><span data-stu-id="6bc3a-274">Add *Models\Contact.cs*:</span></span>

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="6bc3a-275">Scaffold o `Contact` modelo.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-275">Scaffold the `Contact` model.</span></span>
* <span data-ttu-id="6bc3a-276">Criar a migração inicial e atualizar o banco de dados:</span><span class="sxs-lookup"><span data-stu-id="6bc3a-276">Create initial migration and update the database:</span></span>

```console
dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
dotnet ef database drop -f
dotnet ef migrations add initial
dotnet ef database update
```

* <span data-ttu-id="6bc3a-277">Atualizar o link de **ContactManager** no arquivo *Pages/_Layout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="6bc3a-277">Update the **ContactManager** anchor in the *Pages/_Layout.cshtml* file:</span></span>

```cshtml
<a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
```

* <span data-ttu-id="6bc3a-278">Testar o aplicativo criando, editando e excluindo um contato</span><span class="sxs-lookup"><span data-stu-id="6bc3a-278">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="6bc3a-279">Propagar o banco de dados</span><span class="sxs-lookup"><span data-stu-id="6bc3a-279">Seed the database</span></span>

<span data-ttu-id="6bc3a-280">Adicione a [SeedData](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs) de classe para o *dados* pasta.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-280">Add the [SeedData](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs) class to the *Data* folder.</span></span>

<span data-ttu-id="6bc3a-281">Chame `SeedData.Initialize` de `Main`:</span><span class="sxs-lookup"><span data-stu-id="6bc3a-281">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Program.cs?name=snippet)]

<span data-ttu-id="6bc3a-282">Se o aplicativo propagado o banco de dados de teste.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-282">Test that the app seeded the database.</span></span> <span data-ttu-id="6bc3a-283">Se não houver nenhuma linha no banco de dados de contato, o método de propagação não será executado.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-283">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a><span data-ttu-id="6bc3a-284">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="6bc3a-284">Additional resources</span></span>

* <span data-ttu-id="6bc3a-285">[Laboratório de autorização do ASP.NET Core](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span><span class="sxs-lookup"><span data-stu-id="6bc3a-285">[ASP.NET Core Authorization Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span></span> <span data-ttu-id="6bc3a-286">Este laboratório apresenta mais detalhes sobre os recursos de segurança introduzidos neste tutorial.</span><span class="sxs-lookup"><span data-stu-id="6bc3a-286">This lab goes into more detail on the security features introduced in this tutorial.</span></span>
* [<span data-ttu-id="6bc3a-287">Autorização no ASP.NET Core: simples, função, baseada em declarações e personalizada</span><span class="sxs-lookup"><span data-stu-id="6bc3a-287">Authorization in ASP.NET Core: Simple, role, claims-based, and custom</span></span>](xref:security/authorization/index)
* [<span data-ttu-id="6bc3a-288">Autorização baseada em política personalizada</span><span class="sxs-lookup"><span data-stu-id="6bc3a-288">Custom policy-based authorization</span></span>](xref:security/authorization/policies)

::: moniker-end