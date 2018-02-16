---
title: "Criar um aplicativo do ASP.NET Core com dados de usuário protegidos por autorização"
author: rick-anderson
description: "Saiba como criar um aplicativo de páginas Razor com dados de usuário protegidos por autorização. Inclui HTTPS, autenticação, segurança, a identidade do ASP.NET Core."
manager: wpickett
ms.author: riande
ms.date: 01/24/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/secure-data
ms.openlocfilehash: e186adef2e72f852543a92ddce0e82be2a3bcd12
ms.sourcegitcommit: 809ee4baf8bf7b4cae9e366ecae29de1037d2bbb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/15/2018
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a><span data-ttu-id="ca7c0-104">Criar um aplicativo do ASP.NET Core com dados de usuário protegidos por autorização</span><span class="sxs-lookup"><span data-stu-id="ca7c0-104">Create an ASP.NET Core app with user data protected by authorization</span></span>

<span data-ttu-id="ca7c0-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT) e [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="ca7c0-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="ca7c0-106">Este tutorial mostra como criar um aplicativo web do ASP.NET Core com dados de usuário protegidos por autorização.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-106">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="ca7c0-107">Exibe uma lista de contatos (registrados) usuários autenticados criou.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-107">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="ca7c0-108">Há três grupos de segurança:</span><span class="sxs-lookup"><span data-stu-id="ca7c0-108">There are three security groups:</span></span>

* <span data-ttu-id="ca7c0-109">**Usuários registrados** pode exibir todos os dados aprovados e pode editar/excluir seus próprios dados.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-109">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="ca7c0-110">**Gerenciadores de** pode aprovar ou rejeitar dados de contato.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-110">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="ca7c0-111">Apenas os contatos aprovados são visíveis aos usuários.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-111">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="ca7c0-112">**Os administradores** pode rejeitar/aprovar e editar/excluir todos os dados.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-112">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="ca7c0-113">Na imagem a seguir, o usuário Rick (`rick@example.com`) está conectado.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-113">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="ca7c0-114">Rick só pode exibir contatos aprovados e **editar**/**excluir**/**criar novo** links para seus contatos.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-114">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="ca7c0-115">Somente o último registro criado pelo Rick, exibe **editar** e **excluir** links.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-115">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="ca7c0-116">Outros usuários não verão o último registro até que um gerente ou administrador altera o status como "Aprovada".</span><span class="sxs-lookup"><span data-stu-id="ca7c0-116">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![imagem descrita anterior](secure-data/_static/rick.png)

<span data-ttu-id="ca7c0-118">Na imagem a seguir, `manager@contoso.com` está conectado e na função gerenciadores:</span><span class="sxs-lookup"><span data-stu-id="ca7c0-118">In the following image, `manager@contoso.com` is signed in and in the managers role:</span></span>

![imagem descrita anterior](secure-data/_static/manager1.png)

<span data-ttu-id="ca7c0-120">A imagem a seguir mostra os gerentes de modo de exibição de detalhes de um contato:</span><span class="sxs-lookup"><span data-stu-id="ca7c0-120">The following image shows the managers details view of a contact:</span></span>

![imagem descrita anterior](secure-data/_static/manager.png)

<span data-ttu-id="ca7c0-122">O **aprovar** e **rejeitar** botões são exibidos somente para administradores e gerentes.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-122">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="ca7c0-123">Na imagem a seguir, `admin@contoso.com` está conectado e na função de administradores:</span><span class="sxs-lookup"><span data-stu-id="ca7c0-123">In the following image, `admin@contoso.com` is signed in and in the administrators role:</span></span>

![imagem descrita anterior](secure-data/_static/admin.png)

<span data-ttu-id="ca7c0-125">O administrador tem todos os privilégios.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-125">The administrator has all privileges.</span></span> <span data-ttu-id="ca7c0-126">Ela pode ler/editar/excluir qualquer contato e alterar o status de contatos.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-126">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="ca7c0-127">O aplicativo foi criado por [scaffolding](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) o seguinte `Contact` modelo:</span><span class="sxs-lookup"><span data-stu-id="ca7c0-127">The app was created by [scaffolding](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) the following `Contact` model:</span></span>

[!code-csharp[Main](secure-data/samples/starter2/Models/Contact.cs?name=snippet1)]

<span data-ttu-id="ca7c0-128">O exemplo contém os seguintes manipuladores de autorização:</span><span class="sxs-lookup"><span data-stu-id="ca7c0-128">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="ca7c0-129">`ContactIsOwnerAuthorizationHandler`: Garante que um usuário só pode editar seus dados.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-129">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="ca7c0-130">`ContactManagerAuthorizationHandler`: Permite que os gerentes aprovar ou rejeitar contatos.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-130">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="ca7c0-131">`ContactAdministratorsAuthorizationHandler`: Permite aos administradores para aprovar ou rejeitar contatos e editar/excluir contatos.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-131">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ca7c0-132">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="ca7c0-132">Prerequisites</span></span>

<span data-ttu-id="ca7c0-133">Este tutorial é avançado.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-133">This tutorial is advanced.</span></span> <span data-ttu-id="ca7c0-134">Você deve estar familiarizado com:</span><span class="sxs-lookup"><span data-stu-id="ca7c0-134">You should be familiar with:</span></span>

* [<span data-ttu-id="ca7c0-135">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ca7c0-135">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="ca7c0-136">Autenticação</span><span class="sxs-lookup"><span data-stu-id="ca7c0-136">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="ca7c0-137">Confirmação de conta e recuperação de senha</span><span class="sxs-lookup"><span data-stu-id="ca7c0-137">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="ca7c0-138">Autorização</span><span class="sxs-lookup"><span data-stu-id="ca7c0-138">Authorization</span></span>](xref:security/authorization/index)
* [<span data-ttu-id="ca7c0-139">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="ca7c0-139">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<span data-ttu-id="ca7c0-140">Consulte [este arquivo PDF](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) para a versão do MVC do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-140">See [this PDF file](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC version.</span></span> <span data-ttu-id="ca7c0-141">A versão 1.1 do ASP.NET Core deste tutorial está no [isso](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) pasta.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-141">The ASP.NET Core 1.1 version of this tutorial is in [this](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) folder.</span></span> <span data-ttu-id="ca7c0-142">O 1.1 exemplo do ASP.NET Core está no [exemplos](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span><span class="sxs-lookup"><span data-stu-id="ca7c0-142">The 1.1 ASP.NET Core sample is in the [samples](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span></span>

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="ca7c0-143">O início e o aplicativo concluído</span><span class="sxs-lookup"><span data-stu-id="ca7c0-143">The starter and completed app</span></span>

<span data-ttu-id="ca7c0-144">[Baixar](xref:tutorials/index#how-to-download-a-sample) o [concluída](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2) aplicativo.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-144">[Download](xref:tutorials/index#how-to-download-a-sample) the [completed](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2) app.</span></span> <span data-ttu-id="ca7c0-145">[Teste](#test-the-completed-app) o aplicativo concluído para que você se familiarize com seus recursos de segurança.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-145">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="ca7c0-146">O aplicativo de início</span><span class="sxs-lookup"><span data-stu-id="ca7c0-146">The starter app</span></span>

<span data-ttu-id="ca7c0-147">[Baixar](xref:tutorials/index#how-to-download-a-sample) o [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2) aplicativo.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-147">[Download](xref:tutorials/index#how-to-download-a-sample) the [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2) app.</span></span>

<span data-ttu-id="ca7c0-148">Executar o aplicativo, toque o **ContactManager** vincular e verifique se você pode criar, editar e excluir um contato.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-148">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="ca7c0-149">Proteger os dados de usuário</span><span class="sxs-lookup"><span data-stu-id="ca7c0-149">Secure user data</span></span>

<span data-ttu-id="ca7c0-150">As seções a seguir têm todas as principais etapas para criar o aplicativo de dados de usuário segura.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-150">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="ca7c0-151">Talvez seja útil para fazer referência ao projeto concluído.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-151">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="ca7c0-152">Vincular os dados de contato para o usuário</span><span class="sxs-lookup"><span data-stu-id="ca7c0-152">Tie the contact data to the user</span></span>

<span data-ttu-id="ca7c0-153">Usar o ASP.NET [identidade](xref:security/authentication/identity) ID de usuário para garantir que os usuários pode editar seus dados, mas não de outros dados de usuários.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-153">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="ca7c0-154">Adicionar `OwnerID` e `ContactStatus` para o `Contact` modelo:</span><span class="sxs-lookup"><span data-stu-id="ca7c0-154">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

<span data-ttu-id="ca7c0-155">`OwnerID` é a ID do usuário da `AspNetUser` tabela o [identidade](xref:security/authentication/identity) banco de dados.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-155">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="ca7c0-156">O `Status` campo determina se um contato é exibido por usuários gerais.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-156">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="ca7c0-157">Criar uma nova migração e atualizar o banco de dados:</span><span class="sxs-lookup"><span data-stu-id="ca7c0-157">Create a new migration and update the database:</span></span>

```console
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="require-https-and-authenticated-users"></a><span data-ttu-id="ca7c0-158">Exigir HTTPS e os usuários autenticados</span><span class="sxs-lookup"><span data-stu-id="ca7c0-158">Require HTTPS and authenticated users</span></span>

<span data-ttu-id="ca7c0-159">Adicionar [IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) para `Startup`:</span><span class="sxs-lookup"><span data-stu-id="ca7c0-159">Add [IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) to `Startup`:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Startup.cs?name=snippet_env)]

<span data-ttu-id="ca7c0-160">No `ConfigureServices` método o *Startup.cs* de arquivo, adicione o [RequireHttpsAttribute](/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) filtro de autorização:</span><span class="sxs-lookup"><span data-stu-id="ca7c0-160">In the `ConfigureServices` method of the *Startup.cs* file, add the [RequireHttpsAttribute](/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) authorization filter:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Startup.cs?name=snippet_SSL&highlight=10-999)]

<span data-ttu-id="ca7c0-161">Se você estiver usando o Visual Studio, habilite o HTTPS.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-161">If you're using Visual Studio, enable HTTPS.</span></span>

<span data-ttu-id="ca7c0-162">Para redirecionar solicitações HTTP para HTTPS, consulte [Middleware de regravação de URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="ca7c0-162">To redirect HTTP requests to HTTPS, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="ca7c0-163">Se você estiver usando o código do Visual Studio ou testes em uma plataforma local que não inclui um certificado de teste para HTTPS:</span><span class="sxs-lookup"><span data-stu-id="ca7c0-163">If you're using Visual Studio Code or testing on a local platform that doesn't include a test certificate for HTTPS:</span></span>

  <span data-ttu-id="ca7c0-164">Definir `"LocalTest:skipSSL": true` no *appsettings. Developement.JSON* arquivo.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-164">Set `"LocalTest:skipSSL": true` in the *appsettings.Developement.json* file.</span></span>

### <a name="require-authenticated-users"></a><span data-ttu-id="ca7c0-165">Exigir que os usuários autenticados</span><span class="sxs-lookup"><span data-stu-id="ca7c0-165">Require authenticated users</span></span>

<span data-ttu-id="ca7c0-166">Defina a política de autenticação padrão para exigir que os usuários sejam autenticados.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-166">Set the default authentication policy to require users to be authenticated.</span></span> <span data-ttu-id="ca7c0-167">Você pode recusar a autenticação no nível de método página Razor, controlador ou ação com o `[AllowAnonymous]` atributo.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-167">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="ca7c0-168">Configurar a política de autenticação padrão para exigir que os usuários sejam autenticados protege recém-adicionado páginas Razor e controladores.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-168">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="ca7c0-169">Com a autenticação exigida por padrão é mais segura do que contar com novos controladores e páginas Razor para incluir o `[Authorize]` atributo.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-169">Having authentication required by default is safer than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span> 

<span data-ttu-id="ca7c0-170">Com o requisito de todos os usuários autenticados, o [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder?view=aspnetcore-2.0#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) e [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage?view=aspnetcore-2.0) chamadas não são necessárias.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-170">With the requirement of all users authenticated, the [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder?view=aspnetcore-2.0#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) and [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage?view=aspnetcore-2.0) calls are not required.</span></span>

<span data-ttu-id="ca7c0-171">Atualização `ConfigureServices` com as seguintes alterações:</span><span class="sxs-lookup"><span data-stu-id="ca7c0-171">Update `ConfigureServices` with the following changes:</span></span>

* <span data-ttu-id="ca7c0-172">Comente `AuthorizeFolder` e `AuthorizePage`.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-172">Comment out `AuthorizeFolder` and `AuthorizePage`.</span></span>
* <span data-ttu-id="ca7c0-173">Defina a política de autenticação padrão para exigir que os usuários sejam autenticados.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-173">Set the default authentication policy to require users to be authenticated.</span></span>

[!code-csharp[Main](secure-data/samples/final2/Startup.cs?name=snippet_defaultPolicy&highlight=23-27,31-999)]

<span data-ttu-id="ca7c0-174">Adicionar [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) ao índice, sobre e contatos páginas para usuários anônimos podem obter informações sobre o site antes de eles se registrar.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-174">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index, About, and Contact pages so anonymous users can get information about the site before they register.</span></span> 

[!code-csharp[Main](secure-data/samples/final2/Pages/Index.cshtml.cs?name=snippet&highlight=2)]

<span data-ttu-id="ca7c0-175">Adicionar `[AllowAnonymous]` para o [LoginModel e RegisterModel](https://github.com/aspnet/templating/issues/238).</span><span class="sxs-lookup"><span data-stu-id="ca7c0-175">Add `[AllowAnonymous]` to the [LoginModel and RegisterModel](https://github.com/aspnet/templating/issues/238).</span></span>

### <a name="configure-the-test-account"></a><span data-ttu-id="ca7c0-176">Configurar a conta de teste</span><span class="sxs-lookup"><span data-stu-id="ca7c0-176">Configure the test account</span></span>

<span data-ttu-id="ca7c0-177">O `SeedData` classe cria duas contas: administrador e Gerenciador.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-177">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="ca7c0-178">Use o [ferramenta Gerenciador de segredo](xref:security/app-secrets) para definir uma senha para essas contas.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-178">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="ca7c0-179">Definir a senha do diretório do projeto (o diretório que contém *Program.cs*):</span><span class="sxs-lookup"><span data-stu-id="ca7c0-179">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```console
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="ca7c0-180">Atualização `Main` para usar a senha de teste:</span><span class="sxs-lookup"><span data-stu-id="ca7c0-180">Update `Main` to use the test password:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="ca7c0-181">Criar as contas de teste e atualizar os contatos</span><span class="sxs-lookup"><span data-stu-id="ca7c0-181">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="ca7c0-182">Atualização de `Initialize` método o `SeedData` classe para criar as contas de teste:</span><span class="sxs-lookup"><span data-stu-id="ca7c0-182">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="ca7c0-183">Adicione a ID de usuário de administrador e `ContactStatus` para os contatos.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-183">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="ca7c0-184">Tornar um dos contatos "Enviada" e um "rejeitado".</span><span class="sxs-lookup"><span data-stu-id="ca7c0-184">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="ca7c0-185">Adicione a ID de usuário e o status para todos os contatos.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-185">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="ca7c0-186">Somente um contato é exibido:</span><span class="sxs-lookup"><span data-stu-id="ca7c0-186">Only one contact is shown:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="ca7c0-187">Criar proprietário, Gerenciador e manipuladores de autorização do administrador</span><span class="sxs-lookup"><span data-stu-id="ca7c0-187">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="ca7c0-188">Criar um `ContactIsOwnerAuthorizationHandler` classe no *autorização* pasta.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-188">Create a `ContactIsOwnerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="ca7c0-189">O `ContactIsOwnerAuthorizationHandler` verifica que o usuário atuando em um recurso possui o recurso.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-189">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[Main](secure-data/samples/final2/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="ca7c0-190">O `ContactIsOwnerAuthorizationHandler` chamadas [contexto. Êxito](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) se o usuário autenticado atual é o proprietário do contato.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-190">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="ca7c0-191">Manipuladores de autorização geralmente:</span><span class="sxs-lookup"><span data-stu-id="ca7c0-191">Authorization handlers generally:</span></span>

* <span data-ttu-id="ca7c0-192">Retornar `context.Succeed` quando os requisitos são atendidos.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-192">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="ca7c0-193">Retornar `Task.CompletedTask` quando os requisitos não atendidos.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-193">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="ca7c0-194">`Task.CompletedTask` não é êxito ou falha&mdash;permite que outros manipuladores de autorização executar.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-194">`Task.CompletedTask` is neither success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="ca7c0-195">Se você precisar explicitamente falhar, retornar [contexto. Falha](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span><span class="sxs-lookup"><span data-stu-id="ca7c0-195">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="ca7c0-196">O aplicativo permite que proprietários de contato para editar/excluir/criar seus próprios dados.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-196">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="ca7c0-197">`ContactIsOwnerAuthorizationHandler` não precisa verificar a operação passada no parâmetro de requisito.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-197">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="ca7c0-198">Criar um manipulador de autorização do Gerenciador</span><span class="sxs-lookup"><span data-stu-id="ca7c0-198">Create a manager authorization handler</span></span>

<span data-ttu-id="ca7c0-199">Criar um `ContactManagerAuthorizationHandler` classe no *autorização* pasta.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-199">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="ca7c0-200">O `ContactManagerAuthorizationHandler` verifica se o usuário atuando no recurso é um Gerenciador de.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-200">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="ca7c0-201">Somente os gerentes podem aprovar ou rejeitar alterações de conteúdo (nova ou alteradas).</span><span class="sxs-lookup"><span data-stu-id="ca7c0-201">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[Main](secure-data/samples/final2/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="ca7c0-202">Criar um manipulador de autorização do administrador</span><span class="sxs-lookup"><span data-stu-id="ca7c0-202">Create an administrator authorization handler</span></span>

<span data-ttu-id="ca7c0-203">Criar um `ContactAdministratorsAuthorizationHandler` classe no *autorização* pasta.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-203">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="ca7c0-204">O `ContactAdministratorsAuthorizationHandler` verifica se o usuário atuando no recurso é um administrador.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-204">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="ca7c0-205">Administrador pode fazer todas as operações.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-205">Administrator can do all operations.</span></span>

[!code-csharp[Main](secure-data/samples/final2/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="ca7c0-206">Registrar os manipuladores de autorização</span><span class="sxs-lookup"><span data-stu-id="ca7c0-206">Register the authorization handlers</span></span>

<span data-ttu-id="ca7c0-207">Serviços usando o Entity Framework Core devem ser registrados para [injeção de dependência](xref:fundamentals/dependency-injection) usando [AddScoped](/aspnet/core/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span><span class="sxs-lookup"><span data-stu-id="ca7c0-207">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/aspnet/core/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="ca7c0-208">O `ContactIsOwnerAuthorizationHandler` usa o ASP.NET Core [identidade](xref:security/authentication/identity), que está incluído no Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-208">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="ca7c0-209">Registrar os manipuladores com a coleção de serviço para que eles estejam disponíveis para o `ContactsController` por meio de [injeção de dependência](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="ca7c0-209">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="ca7c0-210">Adicione o seguinte código ao final da `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="ca7c0-210">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Startup.cs?name=ConfigureServices&highlight=41-999)]

<span data-ttu-id="ca7c0-211">`ContactAdministratorsAuthorizationHandler` e `ContactManagerAuthorizationHandler` são adicionados como singletons.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-211">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="ca7c0-212">Elas são singletons porque eles não usam o EF e todas as informações necessárias no `Context` parâmetro do `HandleRequirementAsync` método.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-212">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="ca7c0-213">Suporte à autorização</span><span class="sxs-lookup"><span data-stu-id="ca7c0-213">Support authorization</span></span>

<span data-ttu-id="ca7c0-214">Nesta seção, você atualize as páginas Razor e adicionar uma classe de requisitos de operações.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-214">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="ca7c0-215">Examine a classe de requisitos de operações de contato</span><span class="sxs-lookup"><span data-stu-id="ca7c0-215">Review the contact operations requirements class</span></span>

<span data-ttu-id="ca7c0-216">Examine o `ContactOperations` classe.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-216">Review the `ContactOperations` class.</span></span> <span data-ttu-id="ca7c0-217">Essa classe contém os requisitos de aplicativo suporta:</span><span class="sxs-lookup"><span data-stu-id="ca7c0-217">This class contains the requirements the app supports:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-razor-pages"></a><span data-ttu-id="ca7c0-218">Criar uma classe base para as páginas Razor</span><span class="sxs-lookup"><span data-stu-id="ca7c0-218">Create a base class for the Razor Pages</span></span>

<span data-ttu-id="ca7c0-219">Crie uma classe base que contém os serviços usados em contatos páginas Razor.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-219">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="ca7c0-220">A classe base coloca esse código de inicialização em um local:</span><span class="sxs-lookup"><span data-stu-id="ca7c0-220">The base class puts that initialization code in one location:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="ca7c0-221">O código anterior:</span><span class="sxs-lookup"><span data-stu-id="ca7c0-221">The preceding code:</span></span>

* <span data-ttu-id="ca7c0-222">Adiciona o `IAuthorizationService` serviço acesse os manipuladores de autorização.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-222">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="ca7c0-223">Adiciona a identidade `UserManager` serviço.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-223">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="ca7c0-224">Adicione a `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-224">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="ca7c0-225">Atualizar o CreateModel</span><span class="sxs-lookup"><span data-stu-id="ca7c0-225">Update the CreateModel</span></span>

<span data-ttu-id="ca7c0-226">Atualizar o construtor de modelo de página criar para usar o `DI_BasePageModel` classe base:</span><span class="sxs-lookup"><span data-stu-id="ca7c0-226">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="ca7c0-227">Atualização de `CreateModel.OnPostAsync` método:</span><span class="sxs-lookup"><span data-stu-id="ca7c0-227">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="ca7c0-228">Adicione a ID de usuário para o `Contact` modelo.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-228">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="ca7c0-229">Chama o manipulador de autorização para verificar se que o usuário tem permissão para criar contatos.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-229">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="ca7c0-230">Atualizar o IndexModel</span><span class="sxs-lookup"><span data-stu-id="ca7c0-230">Update the IndexModel</span></span>

<span data-ttu-id="ca7c0-231">Atualização de `OnGetAsync` método apenas os contatos aprovados são mostradas a usuários gerais:</span><span class="sxs-lookup"><span data-stu-id="ca7c0-231">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="ca7c0-232">Atualizar o EditModel</span><span class="sxs-lookup"><span data-stu-id="ca7c0-232">Update the EditModel</span></span>

<span data-ttu-id="ca7c0-233">Adicione um manipulador de autorização para verificar se que o usuário possui o contato.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-233">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="ca7c0-234">Como a autorização de recursos está sendo validada, o `[Authorize]` atributo não é suficiente.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-234">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="ca7c0-235">O aplicativo não tiver acesso ao recurso quando atributos são avaliados.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-235">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="ca7c0-236">Autorização baseada em recursos deve ser obrigatória.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-236">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="ca7c0-237">Verificações de devem ser executadas depois que o aplicativo tenha acesso ao recurso, carregá-lo no modelo de página ou carregá-lo dentro do manipulador de si mesmo.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-237">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="ca7c0-238">Frequentemente, você acessar o recurso, passando a chave de recurso.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-238">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="ca7c0-239">Atualizar o DeleteModel</span><span class="sxs-lookup"><span data-stu-id="ca7c0-239">Update the DeleteModel</span></span>

<span data-ttu-id="ca7c0-240">Atualize o modelo de página de exclusão para usar o manipulador de autorização para verificar se que o usuário tem permissão para excluir no contato.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-240">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="ca7c0-241">Injetar o serviço de autorização para os modos de exibição</span><span class="sxs-lookup"><span data-stu-id="ca7c0-241">Inject the authorization service into the views</span></span>

<span data-ttu-id="ca7c0-242">No momento, a interface do usuário mostra editar e excluir links de dados que o usuário não pode modificar.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-242">Currently, the UI shows edit and delete links for data the user can't modify.</span></span> <span data-ttu-id="ca7c0-243">A interface do usuário é fixo, aplicando o manipulador de autorização para os modos de exibição.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-243">The UI is fixed by applying the authorization handler to the views.</span></span>

<span data-ttu-id="ca7c0-244">Injetar o serviço de autorização no *Views/_ViewImports.cshtml* arquivo para que ele esteja disponível para todos os modos de exibição:</span><span class="sxs-lookup"><span data-stu-id="ca7c0-244">Inject the authorization service in the *Views/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[Main](secure-data/samples/final2/Pages/_ViewImports.cshtml?highlight=6-9)]

<span data-ttu-id="ca7c0-245">A marcação anterior adiciona várias `using` instruções.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-245">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="ca7c0-246">Atualização de **editar** e **excluir** links em *Pages/Contacts/Index.cshtml* para elas estão renderizadas somente para usuários com as permissões apropriadas:</span><span class="sxs-lookup"><span data-stu-id="ca7c0-246">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[Main](secure-data/samples/final2/Pages/Contacts/Index.cshtml?highlight=34-36,64-999)]

> [!WARNING]
> <span data-ttu-id="ca7c0-247">Ocultar links de usuários que não tem permissão para alterar os dados, o aplicativo não seguro.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-247">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="ca7c0-248">Ocultar links torna o aplicativo mais amigável exibindo links só é válidas.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-248">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="ca7c0-249">Os usuários podem hack as URLs geradas para chamar editar e excluir operações nos dados que não possuem.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-249">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="ca7c0-250">O Razor de página ou o controlador deve impor verificações de acesso para proteger os dados.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-250">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="ca7c0-251">Detalhes da atualização</span><span class="sxs-lookup"><span data-stu-id="ca7c0-251">Update Details</span></span>

<span data-ttu-id="ca7c0-252">Atualize a exibição de detalhes para que os gerentes podem aprovar ou rejeitar contatos:</span><span class="sxs-lookup"><span data-stu-id="ca7c0-252">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[Main](secure-data/samples/final2/Pages/Contacts/Details.cshtml?range=48-999)]

<span data-ttu-id="ca7c0-253">Atualize o modelo de página de detalhes:</span><span class="sxs-lookup"><span data-stu-id="ca7c0-253">Update the details page model:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="test-the-completed-app"></a><span data-ttu-id="ca7c0-254">Testar o aplicativo concluído</span><span class="sxs-lookup"><span data-stu-id="ca7c0-254">Test the completed app</span></span>

<span data-ttu-id="ca7c0-255">Se você estiver usando o código do Visual Studio ou testes em uma plataforma local que não inclui um certificado de teste para HTTPS:</span><span class="sxs-lookup"><span data-stu-id="ca7c0-255">If you're using Visual Studio Code or testing on a local platform that doesn't include a test certificate for HTTPS:</span></span>

* <span data-ttu-id="ca7c0-256">Definir `"LocalTest:skipSSL": true` no *appsettings. Developement.JSON* arquivo ignorar o requisito de HTTPS.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-256">Set `"LocalTest:skipSSL": true` in the *appsettings.Developement.json* file to skip the HTTPS requirement.</span></span> <span data-ttu-id="ca7c0-257">Skip HTTPS somente em um computador de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-257">Skip HTTPS only on a development machine.</span></span>

<span data-ttu-id="ca7c0-258">Se o aplicativo tiver contatos:</span><span class="sxs-lookup"><span data-stu-id="ca7c0-258">If the app has contacts:</span></span>

* <span data-ttu-id="ca7c0-259">Excluir todos os registros de `Contact` tabela.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-259">Delete all the records in the `Contact` table.</span></span>
* <span data-ttu-id="ca7c0-260">Reinicie o aplicativo para propagar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-260">Restart the app to seed the database.</span></span>

<span data-ttu-id="ca7c0-261">Registre um usuário para procurar os contatos.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-261">Register a user for browsing the contacts.</span></span>

<span data-ttu-id="ca7c0-262">Uma maneira fácil de testar o aplicativo concluído é iniciar três navegadores diferentes (ou versões de incógnita/InPrivate).</span><span class="sxs-lookup"><span data-stu-id="ca7c0-262">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate versions).</span></span> <span data-ttu-id="ca7c0-263">Em um navegador, registrar um novo usuário (por exemplo, `test@example.com`).</span><span class="sxs-lookup"><span data-stu-id="ca7c0-263">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="ca7c0-264">Entrar para cada localizador com um usuário diferente.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-264">Sign in to each browser with a different user.</span></span> <span data-ttu-id="ca7c0-265">Verifique se as seguintes operações:</span><span class="sxs-lookup"><span data-stu-id="ca7c0-265">Verify the following operations:</span></span>

* <span data-ttu-id="ca7c0-266">Os usuários registrados podem exibir todos os dados de contato aprovados.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-266">Registered users can view all the approved contact data.</span></span>
* <span data-ttu-id="ca7c0-267">Os usuários registrados podem editar/excluir seus próprios dados.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-267">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="ca7c0-268">Os gerentes podem aprovar ou rejeitar dados de contato.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-268">Managers can approve or reject contact data.</span></span> <span data-ttu-id="ca7c0-269">O `Details` exibição mostra **aprovar** e **rejeitar** botões.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-269">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="ca7c0-270">Os administradores podem Aprovar/rejeitar e editar/excluir todos os dados.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-270">Administrators can approve/reject and edit/delete any data.</span></span>

| <span data-ttu-id="ca7c0-271">User</span><span class="sxs-lookup"><span data-stu-id="ca7c0-271">User</span></span>| <span data-ttu-id="ca7c0-272">Opções</span><span class="sxs-lookup"><span data-stu-id="ca7c0-272">Options</span></span> |
| ------------ | ---------|
| test@example.com | <span data-ttu-id="ca7c0-273">Pode editar/excluir próprios dados</span><span class="sxs-lookup"><span data-stu-id="ca7c0-273">Can edit/delete own data</span></span> |
| manager@contoso.com | <span data-ttu-id="ca7c0-274">Rejeitar/aprovar e editar/excluir podem ter dados</span><span class="sxs-lookup"><span data-stu-id="ca7c0-274">Can approve/reject and edit/delete own data</span></span> |
| admin@contoso.com | <span data-ttu-id="ca7c0-275">Pode editar/excluir e Aprovar/rejeitar todos os dados</span><span class="sxs-lookup"><span data-stu-id="ca7c0-275">Can edit/delete and approve/reject all data</span></span>|

<span data-ttu-id="ca7c0-276">Crie um contato no navegador do administrador.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-276">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="ca7c0-277">Copie a URL para excluir e editar o contato de administrador.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-277">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="ca7c0-278">Cole esses links no navegador do usuário de teste para verificar se que o usuário de teste não pode executar essas operações.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-278">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="ca7c0-279">Criar o aplicativo de início</span><span class="sxs-lookup"><span data-stu-id="ca7c0-279">Create the starter app</span></span>

* <span data-ttu-id="ca7c0-280">Criar um aplicativo de páginas Razor denominado "ContactManager"</span><span class="sxs-lookup"><span data-stu-id="ca7c0-280">Create a Razor Pages app named "ContactManager"</span></span>

  * <span data-ttu-id="ca7c0-281">Criar o aplicativo com **contas de usuário individuais**.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-281">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="ca7c0-282">O nome "ContactManager" para o espaço para nome coincida com o namespace usado no exemplo.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-282">Name it "ContactManager" so your namespace matches the namespace used in the sample.</span></span>

  ```console
  dotnet new razor -o ContactManager -au Individual -uld
  ```

  * <span data-ttu-id="ca7c0-283">`-uld` Especifica o LocalDB, em vez de SQLite</span><span class="sxs-lookup"><span data-stu-id="ca7c0-283">`-uld` specifies LocalDB instead of SQLite</span></span>

* <span data-ttu-id="ca7c0-284">Adicione o seguinte `Contact` modelo:</span><span class="sxs-lookup"><span data-stu-id="ca7c0-284">Add the following `Contact` model:</span></span>

  [!code-csharp[Main](secure-data/samples/starter2/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="ca7c0-285">Scaffold o `Contact` modelo:</span><span class="sxs-lookup"><span data-stu-id="ca7c0-285">Scaffold the `Contact` model:</span></span>

```console
dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
```

* <span data-ttu-id="ca7c0-286">Atualização de **ContactManager** fixar no *Pages/_Layout.cshtml* arquivo:</span><span class="sxs-lookup"><span data-stu-id="ca7c0-286">Update the **ContactManager** anchor in the *Pages/_Layout.cshtml* file:</span></span>

```cshtml
<a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
```

* <span data-ttu-id="ca7c0-287">Scaffold a migração inicial e atualizar o banco de dados:</span><span class="sxs-lookup"><span data-stu-id="ca7c0-287">Scaffold the initial migration and update the database:</span></span>

```console
dotnet ef migrations add initial
dotnet ef database update
```

* <span data-ttu-id="ca7c0-288">Testar o aplicativo de criação, edição e exclusão de um contato</span><span class="sxs-lookup"><span data-stu-id="ca7c0-288">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="ca7c0-289">Propagar o banco de dados</span><span class="sxs-lookup"><span data-stu-id="ca7c0-289">Seed the database</span></span>

<span data-ttu-id="ca7c0-290">Adicionar o `SeedData` de classe para o *dados* pasta.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-290">Add the `SeedData` class to the *Data* folder.</span></span> <span data-ttu-id="ca7c0-291">Se você baixou o exemplo, você pode copiar o *SeedData.cs* o arquivo para o *dados* pasta do projeto starter.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-291">If you've downloaded the sample, you can copy the *SeedData.cs* file to the *Data* folder of the starter project.</span></span>

<span data-ttu-id="ca7c0-292">Chamar `SeedData.Initialize` de `Main`:</span><span class="sxs-lookup"><span data-stu-id="ca7c0-292">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[Main](secure-data/samples/starter2/Program.cs?name=snippet)]

<span data-ttu-id="ca7c0-293">Teste o aplicativo propagado o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-293">Test that the app seeded the database.</span></span> <span data-ttu-id="ca7c0-294">Se houver linhas no banco de dados de contato, o método de propagação não é executado.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-294">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a><span data-ttu-id="ca7c0-295">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="ca7c0-295">Additional resources</span></span>

* <span data-ttu-id="ca7c0-296">[Laboratório de autorização de ASP.NET Core](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span><span class="sxs-lookup"><span data-stu-id="ca7c0-296">[ASP.NET Core Authorization Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span></span> <span data-ttu-id="ca7c0-297">Este laboratório apresenta mais detalhes sobre os recursos de segurança introduzidos neste tutorial.</span><span class="sxs-lookup"><span data-stu-id="ca7c0-297">This lab goes into more detail on the security features introduced in this tutorial.</span></span>
* [<span data-ttu-id="ca7c0-298">Autorização no ASP.NET Core: Simple, função, baseada em declarações e personalizada</span><span class="sxs-lookup"><span data-stu-id="ca7c0-298">Authorization in ASP.NET Core: Simple, role, claims-based, and custom</span></span>](xref:security/authorization/index)
* [<span data-ttu-id="ca7c0-299">Autorização baseada em política personalizada</span><span class="sxs-lookup"><span data-stu-id="ca7c0-299">Custom policy-based authorization</span></span>](xref:security/authorization/policies)
