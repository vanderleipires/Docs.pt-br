---
title: Criar um aplicativo do ASP.NET Core com dados de usuário protegidos por autorização
author: rick-anderson
description: Saiba como criar um aplicativo de páginas Razor com dados de usuário protegidos por autorização. Inclui HTTPS, autenticação, segurança, a identidade do ASP.NET Core.
ms.author: riande
ms.date: 01/24/2018
uid: security/authorization/secure-data
ms.openlocfilehash: 32ced054ba559e66de4a300137d56b7c81a4149b
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276374"
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a><span data-ttu-id="79d97-104">Criar um aplicativo do ASP.NET Core com dados de usuário protegidos por autorização</span><span class="sxs-lookup"><span data-stu-id="79d97-104">Create an ASP.NET Core app with user data protected by authorization</span></span>

<span data-ttu-id="79d97-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT) e [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="79d97-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="79d97-106">Este tutorial mostra como criar um aplicativo web do ASP.NET Core com dados de usuário protegidos por autorização.</span><span class="sxs-lookup"><span data-stu-id="79d97-106">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="79d97-107">Ele exibe uma lista de contatos criada por usuários autenticados (registrados).</span><span class="sxs-lookup"><span data-stu-id="79d97-107">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="79d97-108">Há três grupos de segurança:</span><span class="sxs-lookup"><span data-stu-id="79d97-108">There are three security groups:</span></span>

* <span data-ttu-id="79d97-109">**Usuários registrados** podem exibir todos os dados aprovados e podem editar/excluir seus próprios dados.</span><span class="sxs-lookup"><span data-stu-id="79d97-109">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="79d97-110">**Gerenciadores de** podem aprovar ou rejeitar dados de um contato.</span><span class="sxs-lookup"><span data-stu-id="79d97-110">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="79d97-111">Apenas os contatos aprovados são visíveis aos usuários.</span><span class="sxs-lookup"><span data-stu-id="79d97-111">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="79d97-112">**Os administradores** podem rejeitar/aprovar e editar/excluir todos os dados.</span><span class="sxs-lookup"><span data-stu-id="79d97-112">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="79d97-113">Na imagem a seguir, o usuário Rick (`rick@example.com`) está conectado.</span><span class="sxs-lookup"><span data-stu-id="79d97-113">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="79d97-114">Rick só pode ver os contatos aprovados e os links **editar**/**excluir**/**criar novo** de seus contatos.</span><span class="sxs-lookup"><span data-stu-id="79d97-114">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="79d97-115">Somente o último registro criado por Rick exibe os links **editar** e **excluir**.</span><span class="sxs-lookup"><span data-stu-id="79d97-115">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="79d97-116">Outros usuários não verão o último registro até que um gerente ou administrador altere o status para "Aprovado".</span><span class="sxs-lookup"><span data-stu-id="79d97-116">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![imagem descrita anterior](secure-data/_static/rick.png)

<span data-ttu-id="79d97-118">Na imagem a seguir, `manager@contoso.com` está conectado e na função de gerenciadores:</span><span class="sxs-lookup"><span data-stu-id="79d97-118">In the following image, `manager@contoso.com` is signed in and in the managers role:</span></span>

![imagem descrita anterior](secure-data/_static/manager1.png)

<span data-ttu-id="79d97-120">A imagem a seguir mostra a tela de exibição de detalhes de um contato dos gerentes:</span><span class="sxs-lookup"><span data-stu-id="79d97-120">The following image shows the managers details view of a contact:</span></span>

![imagem descrita anterior](secure-data/_static/manager.png)

<span data-ttu-id="79d97-122">O botões **aprovar** e **rejeitar** são exibidos somente para administradores e gerentes.</span><span class="sxs-lookup"><span data-stu-id="79d97-122">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="79d97-123">Na imagem a seguir, `admin@contoso.com` está conectado e na função de gerenciadores:</span><span class="sxs-lookup"><span data-stu-id="79d97-123">In the following image, `admin@contoso.com` is signed in and in the administrators role:</span></span>

![imagem descrita anterior](secure-data/_static/admin.png)

<span data-ttu-id="79d97-125">O administrador tem todos os privilégios.</span><span class="sxs-lookup"><span data-stu-id="79d97-125">The administrator has all privileges.</span></span> <span data-ttu-id="79d97-126">Ele pode ler/editar/excluir todos os contatos e alterar os status deles.</span><span class="sxs-lookup"><span data-stu-id="79d97-126">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="79d97-127">O aplicativo foi criado fazendo [scaffolding](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) do seguinte modelo de `Contact`:</span><span class="sxs-lookup"><span data-stu-id="79d97-127">The app was created by [scaffolding](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) the following `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/starter2/Models/Contact.cs?name=snippet1)]

<span data-ttu-id="79d97-128">O exemplo contém os seguintes manipuladores de autorização:</span><span class="sxs-lookup"><span data-stu-id="79d97-128">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="79d97-129">`ContactIsOwnerAuthorizationHandler`: Garante que um usuário só pode editar seus dados.</span><span class="sxs-lookup"><span data-stu-id="79d97-129">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="79d97-130">`ContactManagerAuthorizationHandler`: Permite que os gerentes aprovem ou rejeitem contatos.</span><span class="sxs-lookup"><span data-stu-id="79d97-130">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="79d97-131">`ContactAdministratorsAuthorizationHandler`: Permite aos administradores aprovar, rejeitar e editar/excluir contatos.</span><span class="sxs-lookup"><span data-stu-id="79d97-131">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="79d97-132">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="79d97-132">Prerequisites</span></span>

<span data-ttu-id="79d97-133">Este tutorial é avançado.</span><span class="sxs-lookup"><span data-stu-id="79d97-133">This tutorial is advanced.</span></span> <span data-ttu-id="79d97-134">Você deve estar familiarizado com:</span><span class="sxs-lookup"><span data-stu-id="79d97-134">You should be familiar with:</span></span>

* [<span data-ttu-id="79d97-135">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="79d97-135">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="79d97-136">Autenticação</span><span class="sxs-lookup"><span data-stu-id="79d97-136">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="79d97-137">Confirmação de conta e recuperação de senha</span><span class="sxs-lookup"><span data-stu-id="79d97-137">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="79d97-138">Autorização</span><span class="sxs-lookup"><span data-stu-id="79d97-138">Authorization</span></span>](xref:security/authorization/index)
* [<span data-ttu-id="79d97-139">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="79d97-139">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<span data-ttu-id="79d97-140">Consulte [este arquivo PDF](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) para a versão do MVC do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="79d97-140">See [this PDF file](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC version.</span></span> <span data-ttu-id="79d97-141">A versão 1.1 do ASP.NET Core deste tutorial está [nesta](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) pasta.</span><span class="sxs-lookup"><span data-stu-id="79d97-141">The ASP.NET Core 1.1 version of this tutorial is in [this](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) folder.</span></span> <span data-ttu-id="79d97-142">O exemplo do ASP.NET Core 1.1 está em [exemplos](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span><span class="sxs-lookup"><span data-stu-id="79d97-142">The 1.1 ASP.NET Core sample is in the [samples](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span></span>

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="79d97-143">O aplicativo inicial e o concluído</span><span class="sxs-lookup"><span data-stu-id="79d97-143">The starter and completed app</span></span>

<span data-ttu-id="79d97-144">[Baixar](xref:tutorials/index#how-to-download-a-sample) o [concluída](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2) aplicativo.</span><span class="sxs-lookup"><span data-stu-id="79d97-144">[Download](xref:tutorials/index#how-to-download-a-sample) the [completed](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2) app.</span></span> <span data-ttu-id="79d97-145">[Teste](#test-the-completed-app) o aplicativo concluído para que você se familiarize com seus recursos de segurança.</span><span class="sxs-lookup"><span data-stu-id="79d97-145">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="79d97-146">O aplicativo inicial</span><span class="sxs-lookup"><span data-stu-id="79d97-146">The starter app</span></span>

<span data-ttu-id="79d97-147">[Baixe](xref:tutorials/index#how-to-download-a-sample) o aplicativo [inicial](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2).</span><span class="sxs-lookup"><span data-stu-id="79d97-147">[Download](xref:tutorials/index#how-to-download-a-sample) the [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2) app.</span></span>

<span data-ttu-id="79d97-148">Execute o aplicativo, clique em **ContactManager** e verifique se você pode criar, editar e excluir um contato.</span><span class="sxs-lookup"><span data-stu-id="79d97-148">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="79d97-149">Proteger os dados de usuário</span><span class="sxs-lookup"><span data-stu-id="79d97-149">Secure user data</span></span>

<span data-ttu-id="79d97-150">As seções a seguir têm todas as principais etapas para criar um aplicativo com dados de usuários seguros.</span><span class="sxs-lookup"><span data-stu-id="79d97-150">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="79d97-151">Talvez seja útil para fazer referência ao projeto concluído.</span><span class="sxs-lookup"><span data-stu-id="79d97-151">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="79d97-152">Vincular os dados de contato ao usuário</span><span class="sxs-lookup"><span data-stu-id="79d97-152">Tie the contact data to the user</span></span>

<span data-ttu-id="79d97-153">Usar o ASP.NET [identidade](xref:security/authentication/identity) ID de usuário para garantir que os usuários pode editar seus dados, mas não de outros dados de usuários.</span><span class="sxs-lookup"><span data-stu-id="79d97-153">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="79d97-154">Adicionar `OwnerID` e `ContactStatus` para o `Contact` modelo:</span><span class="sxs-lookup"><span data-stu-id="79d97-154">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/final2/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

<span data-ttu-id="79d97-155">`OwnerID` é a ID do usuário da tabela `AspNetUser` no banco de dados de [identidade](xref:security/authentication/identity).</span><span class="sxs-lookup"><span data-stu-id="79d97-155">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="79d97-156">O campo `Status` determina se um contato pode ser visto por usuários gerais.</span><span class="sxs-lookup"><span data-stu-id="79d97-156">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="79d97-157">Criar uma nova migração e atualizar o banco de dados:</span><span class="sxs-lookup"><span data-stu-id="79d97-157">Create a new migration and update the database:</span></span>

```console
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="require-https-and-authenticated-users"></a><span data-ttu-id="79d97-158">Exigir HTTPS e usuários autenticados</span><span class="sxs-lookup"><span data-stu-id="79d97-158">Require HTTPS and authenticated users</span></span>

<span data-ttu-id="79d97-159">Adicionar [IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) para `Startup`:</span><span class="sxs-lookup"><span data-stu-id="79d97-159">Add [IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) to `Startup`:</span></span>

[!code-csharp[](secure-data/samples/final2/Startup.cs?name=snippet_env)]

<span data-ttu-id="79d97-160">No método `ConfigureServices` do arquivo *Startup.cs*  , adicione o filtro de autorização [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute):</span><span class="sxs-lookup"><span data-stu-id="79d97-160">In the `ConfigureServices` method of the *Startup.cs* file, add the [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) authorization filter:</span></span>

[!code-csharp[](secure-data/samples/final2/Startup.cs?name=snippet_SSL&highlight=10-999)]

<span data-ttu-id="79d97-161">Se você estiver usando o Visual Studio, habilite o HTTPS.</span><span class="sxs-lookup"><span data-stu-id="79d97-161">If you're using Visual Studio, enable HTTPS.</span></span>

<span data-ttu-id="79d97-162">Para redirecionar solicitações HTTP para HTTPS, consulte [Middleware de regravação de URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="79d97-162">To redirect HTTP requests to HTTPS, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="79d97-163">Se você estiver usando o Visual Studio Code ou testando em uma plataforma local que não inclui um certificado de teste para HTTPS:</span><span class="sxs-lookup"><span data-stu-id="79d97-163">If you're using Visual Studio Code or testing on a local platform that doesn't include a test certificate for HTTPS:</span></span>

  <span data-ttu-id="79d97-164">Definir `"LocalTest:skipHTTPS": true` no *appsettings. Developement.JSON* arquivo.</span><span class="sxs-lookup"><span data-stu-id="79d97-164">Set `"LocalTest:skipHTTPS": true` in the *appsettings.Developement.json* file.</span></span>

### <a name="require-authenticated-users"></a><span data-ttu-id="79d97-165">Exigir usuários autenticados</span><span class="sxs-lookup"><span data-stu-id="79d97-165">Require authenticated users</span></span>

<span data-ttu-id="79d97-166">Defina a política de autenticação padrão para exigir que os usuários sejam autenticados.</span><span class="sxs-lookup"><span data-stu-id="79d97-166">Set the default authentication policy to require users to be authenticated.</span></span> <span data-ttu-id="79d97-167">Você pode recusar a autenticação no nível de método página Razor, controlador ou ação com o `[AllowAnonymous]` atributo.</span><span class="sxs-lookup"><span data-stu-id="79d97-167">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="79d97-168">Configurar a política de autenticação padrão para exigir que os usuários sejam autenticados protege recém-adicionado páginas Razor e controladores.</span><span class="sxs-lookup"><span data-stu-id="79d97-168">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="79d97-169">Com a autenticação exigida por padrão é mais segura do que contar com novos controladores e páginas Razor para incluir o `[Authorize]` atributo.</span><span class="sxs-lookup"><span data-stu-id="79d97-169">Having authentication required by default is safer than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span> 

<span data-ttu-id="79d97-170">Com o requisito de todos os usuários autenticados, o [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder?view=aspnetcore-2.0#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) e [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage?view=aspnetcore-2.0) chamadas não são necessárias.</span><span class="sxs-lookup"><span data-stu-id="79d97-170">With the requirement of all users authenticated, the [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder?view=aspnetcore-2.0#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) and [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage?view=aspnetcore-2.0) calls are not required.</span></span>

<span data-ttu-id="79d97-171">Atualize `ConfigureServices` com as seguintes alterações:</span><span class="sxs-lookup"><span data-stu-id="79d97-171">Update `ConfigureServices` with the following changes:</span></span>

* <span data-ttu-id="79d97-172">Comente `AuthorizeFolder` e `AuthorizePage`.</span><span class="sxs-lookup"><span data-stu-id="79d97-172">Comment out `AuthorizeFolder` and `AuthorizePage`.</span></span>
* <span data-ttu-id="79d97-173">Defina a política de autenticação padrão para exigir que os usuários sejam autenticados.</span><span class="sxs-lookup"><span data-stu-id="79d97-173">Set the default authentication policy to require users to be authenticated.</span></span>

[!code-csharp[](secure-data/samples/final2/Startup.cs?name=snippet_defaultPolicy&highlight=23-27,31-999)]

<span data-ttu-id="79d97-174">Adicionar [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute)às páginas Índice, Sobre e Contatos para que usuários anônimos possam obter informações sobre o site antes de eles se registrarem.</span><span class="sxs-lookup"><span data-stu-id="79d97-174">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index, About, and Contact pages so anonymous users can get information about the site before they register.</span></span> 

[!code-csharp[](secure-data/samples/final2/Pages/Index.cshtml.cs?name=snippet&highlight=2)]

<span data-ttu-id="79d97-175">Adicionar `[AllowAnonymous]` para o [LoginModel e RegisterModel](https://github.com/aspnet/templating/issues/238).</span><span class="sxs-lookup"><span data-stu-id="79d97-175">Add `[AllowAnonymous]` to the [LoginModel and RegisterModel](https://github.com/aspnet/templating/issues/238).</span></span>

### <a name="configure-the-test-account"></a><span data-ttu-id="79d97-176">Configurar a conta de teste</span><span class="sxs-lookup"><span data-stu-id="79d97-176">Configure the test account</span></span>

<span data-ttu-id="79d97-177">A classe `SeedData` cria duas contas: administrador e gerenciador.</span><span class="sxs-lookup"><span data-stu-id="79d97-177">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="79d97-178">Use a [ferramenta Gerenciador de segredo](xref:security/app-secrets) para definir uma senha para essas contas.</span><span class="sxs-lookup"><span data-stu-id="79d97-178">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="79d97-179">Defina a senha do diretório do projeto (o diretório que contém *Program.cs*):</span><span class="sxs-lookup"><span data-stu-id="79d97-179">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```console
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="79d97-180">Se você não usar uma senha forte, uma exceção é lançada quando `SeedData.Initialize` é chamado.</span><span class="sxs-lookup"><span data-stu-id="79d97-180">If you don't use a strong password, an exception is thrown when `SeedData.Initialize` is called.</span></span>

<span data-ttu-id="79d97-181">Atualização `Main` para usar a senha de teste:</span><span class="sxs-lookup"><span data-stu-id="79d97-181">Update `Main` to use the test password:</span></span>

[!code-csharp[](secure-data/samples/final2/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="79d97-182">Criar as contas de teste e atualizar os contatos</span><span class="sxs-lookup"><span data-stu-id="79d97-182">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="79d97-183">Atualize o método `Initialize` na classe `SeedData` para criar as contas de teste:</span><span class="sxs-lookup"><span data-stu-id="79d97-183">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[](secure-data/samples/final2/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="79d97-184">Adicione a ID de usuário de administrador e `ContactStatus` aos contatos.</span><span class="sxs-lookup"><span data-stu-id="79d97-184">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="79d97-185">Torne um dos contatos "Enviado" e um "Rejeitado".</span><span class="sxs-lookup"><span data-stu-id="79d97-185">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="79d97-186">Adicione a ID de usuário e o status para todos os contatos.</span><span class="sxs-lookup"><span data-stu-id="79d97-186">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="79d97-187">Somente um contato é exibido:</span><span class="sxs-lookup"><span data-stu-id="79d97-187">Only one contact is shown:</span></span>

[!code-csharp[](secure-data/samples/final2/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="79d97-188">Criar proprietário, Gerenciador e manipuladores de autorização do administrador</span><span class="sxs-lookup"><span data-stu-id="79d97-188">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="79d97-189">Criar uma classe `ContactIsOwnerAuthorizationHandler` na pasta *autorização*.</span><span class="sxs-lookup"><span data-stu-id="79d97-189">Create a `ContactIsOwnerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="79d97-190">O `ContactIsOwnerAuthorizationHandler` verifica que o usuário atuando em um recurso possui o recurso.</span><span class="sxs-lookup"><span data-stu-id="79d97-190">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[](secure-data/samples/final2/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="79d97-191">O `ContactIsOwnerAuthorizationHandler` chamadas [contexto. Êxito](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) se o usuário autenticado atual é o proprietário do contato.</span><span class="sxs-lookup"><span data-stu-id="79d97-191">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="79d97-192">Manipuladores de autorização geralmente:</span><span class="sxs-lookup"><span data-stu-id="79d97-192">Authorization handlers generally:</span></span>

* <span data-ttu-id="79d97-193">Retornar `context.Succeed` quando os requisitos são atendidos.</span><span class="sxs-lookup"><span data-stu-id="79d97-193">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="79d97-194">Retornar `Task.CompletedTask` quando os requisitos não atendidos.</span><span class="sxs-lookup"><span data-stu-id="79d97-194">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="79d97-195">`Task.CompletedTask` não é êxito ou falha&mdash;permite que outros manipuladores de autorização executar.</span><span class="sxs-lookup"><span data-stu-id="79d97-195">`Task.CompletedTask` is neither success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="79d97-196">Se você precisar explicitamente falhar, retornar [contexto. Falha](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span><span class="sxs-lookup"><span data-stu-id="79d97-196">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="79d97-197">O aplicativo permite que proprietários de contato possam editar/excluir/criar seus dados.</span><span class="sxs-lookup"><span data-stu-id="79d97-197">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="79d97-198">`ContactIsOwnerAuthorizationHandler` não precisa verificar a operação passada no parâmetro de requisito.</span><span class="sxs-lookup"><span data-stu-id="79d97-198">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="79d97-199">Criar um manipulador de autorização do Gerenciador</span><span class="sxs-lookup"><span data-stu-id="79d97-199">Create a manager authorization handler</span></span>

<span data-ttu-id="79d97-200">Criar uma classe `ContactManagerAuthorizationHandler` na pasta *autorização*.</span><span class="sxs-lookup"><span data-stu-id="79d97-200">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="79d97-201">O `ContactManagerAuthorizationHandler` verifica se o usuário atuando no recurso é um Gerenciador de.</span><span class="sxs-lookup"><span data-stu-id="79d97-201">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="79d97-202">Somente os gerentes podem aprovar ou rejeitar alterações de conteúdo (nova ou alteradas).</span><span class="sxs-lookup"><span data-stu-id="79d97-202">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[](secure-data/samples/final2/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="79d97-203">Criar um manipulador de autorização do administrador</span><span class="sxs-lookup"><span data-stu-id="79d97-203">Create an administrator authorization handler</span></span>

<span data-ttu-id="79d97-204">Criar uma classe `ContactAdministratorsAuthorizationHandler` na pasta *autorização*.</span><span class="sxs-lookup"><span data-stu-id="79d97-204">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="79d97-205">O `ContactAdministratorsAuthorizationHandler` verifica se o usuário atuando no recurso é um administrador.</span><span class="sxs-lookup"><span data-stu-id="79d97-205">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="79d97-206">Administrador pode fazer todas as operações.</span><span class="sxs-lookup"><span data-stu-id="79d97-206">Administrator can do all operations.</span></span>

[!code-csharp[](secure-data/samples/final2/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="79d97-207">Registrar os manipuladores de autorização</span><span class="sxs-lookup"><span data-stu-id="79d97-207">Register the authorization handlers</span></span>

<span data-ttu-id="79d97-208">Serviços usando o Entity Framework Core devem ser registrados para [injeção de dependência](xref:fundamentals/dependency-injection) usando [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span><span class="sxs-lookup"><span data-stu-id="79d97-208">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="79d97-209">O `ContactIsOwnerAuthorizationHandler` usa o ASP.NET Core [identidade](xref:security/authentication/identity), que está incluído no Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="79d97-209">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="79d97-210">Registrar os manipuladores com a coleção de serviço para que eles estejam disponíveis para o `ContactsController` por meio de [injeção de dependência](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="79d97-210">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="79d97-211">Adicione o seguinte código ao final da `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="79d97-211">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[](secure-data/samples/final2/Startup.cs?name=ConfigureServices&highlight=41-999)]

<span data-ttu-id="79d97-212">`ContactAdministratorsAuthorizationHandler` e `ContactManagerAuthorizationHandler` são adicionados como singletons.</span><span class="sxs-lookup"><span data-stu-id="79d97-212">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="79d97-213">Elas são singletons porque eles não usam o EF e todas as informações necessárias no `Context` parâmetro do `HandleRequirementAsync` método.</span><span class="sxs-lookup"><span data-stu-id="79d97-213">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="79d97-214">Suporte à autorização</span><span class="sxs-lookup"><span data-stu-id="79d97-214">Support authorization</span></span>

<span data-ttu-id="79d97-215">Nesta seção, você atualize as páginas Razor e adicionar uma classe de requisitos de operações.</span><span class="sxs-lookup"><span data-stu-id="79d97-215">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="79d97-216">Examine a classe de requisitos de operações de contato</span><span class="sxs-lookup"><span data-stu-id="79d97-216">Review the contact operations requirements class</span></span>

<span data-ttu-id="79d97-217">Examine o `ContactOperations` classe.</span><span class="sxs-lookup"><span data-stu-id="79d97-217">Review the `ContactOperations` class.</span></span> <span data-ttu-id="79d97-218">Essa classe contém os requisitos de aplicativo suporta:</span><span class="sxs-lookup"><span data-stu-id="79d97-218">This class contains the requirements the app supports:</span></span>

[!code-csharp[](secure-data/samples/final2/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-razor-pages"></a><span data-ttu-id="79d97-219">Criar uma classe base para as páginas Razor</span><span class="sxs-lookup"><span data-stu-id="79d97-219">Create a base class for the Razor Pages</span></span>

<span data-ttu-id="79d97-220">Crie uma classe base que contém os serviços usados em contatos páginas Razor.</span><span class="sxs-lookup"><span data-stu-id="79d97-220">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="79d97-221">A classe base coloca esse código de inicialização em um local:</span><span class="sxs-lookup"><span data-stu-id="79d97-221">The base class puts that initialization code in one location:</span></span>

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="79d97-222">O código anterior:</span><span class="sxs-lookup"><span data-stu-id="79d97-222">The preceding code:</span></span>

* <span data-ttu-id="79d97-223">Adiciona o serviço `IAuthorizationService` para acessar os manipuladores de autorização.</span><span class="sxs-lookup"><span data-stu-id="79d97-223">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="79d97-224">Adiciona o serviço de identidade `UserManager`.</span><span class="sxs-lookup"><span data-stu-id="79d97-224">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="79d97-225">Adicione a `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="79d97-225">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="79d97-226">Atualizar o CreateModel</span><span class="sxs-lookup"><span data-stu-id="79d97-226">Update the CreateModel</span></span>

<span data-ttu-id="79d97-227">Atualize o construtor de criar modelo de página para usar a classe base `DI_BasePageModel`:</span><span class="sxs-lookup"><span data-stu-id="79d97-227">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="79d97-228">Atualize o método `CreateModel.OnPostAsync` para:</span><span class="sxs-lookup"><span data-stu-id="79d97-228">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="79d97-229">Adicione a ID de usuário ao modelo `Contact`.</span><span class="sxs-lookup"><span data-stu-id="79d97-229">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="79d97-230">Chama o manipulador de autorização para verificar se que o usuário tem permissão para criar contatos.</span><span class="sxs-lookup"><span data-stu-id="79d97-230">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="79d97-231">Atualizar o IndexModel</span><span class="sxs-lookup"><span data-stu-id="79d97-231">Update the IndexModel</span></span>

<span data-ttu-id="79d97-232">Atualize o método `OnGetAsync` para que apenas os contatos aprovados sejam mostrados aos usuários gerais:</span><span class="sxs-lookup"><span data-stu-id="79d97-232">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="79d97-233">Atualizar o EditModel</span><span class="sxs-lookup"><span data-stu-id="79d97-233">Update the EditModel</span></span>

<span data-ttu-id="79d97-234">Adicione um manipulador de autorização para verificar se que o usuário possui o contato.</span><span class="sxs-lookup"><span data-stu-id="79d97-234">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="79d97-235">Como a autorização de recursos está sendo validada, o `[Authorize]` atributo não é suficiente.</span><span class="sxs-lookup"><span data-stu-id="79d97-235">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="79d97-236">O aplicativo não tiver acesso ao recurso quando atributos são avaliados.</span><span class="sxs-lookup"><span data-stu-id="79d97-236">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="79d97-237">Autorização baseada em recursos deve ser obrigatória.</span><span class="sxs-lookup"><span data-stu-id="79d97-237">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="79d97-238">Verificações de devem ser executadas depois que o aplicativo tenha acesso ao recurso, carregá-lo no modelo de página ou carregá-lo dentro do manipulador de si mesmo.</span><span class="sxs-lookup"><span data-stu-id="79d97-238">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="79d97-239">Frequentemente, você acessar o recurso, passando a chave de recurso.</span><span class="sxs-lookup"><span data-stu-id="79d97-239">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="79d97-240">Atualizar o DeleteModel</span><span class="sxs-lookup"><span data-stu-id="79d97-240">Update the DeleteModel</span></span>

<span data-ttu-id="79d97-241">Atualize o modelo de página de exclusão para usar o manipulador de autorização para verificar se que o usuário tem permissão para excluir no contato.</span><span class="sxs-lookup"><span data-stu-id="79d97-241">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="79d97-242">Injetar o serviço de autorização para os modos de exibição</span><span class="sxs-lookup"><span data-stu-id="79d97-242">Inject the authorization service into the views</span></span>

<span data-ttu-id="79d97-243">No momento, a interface do usuário mostra editar e excluir links de dados que o usuário não pode modificar.</span><span class="sxs-lookup"><span data-stu-id="79d97-243">Currently, the UI shows edit and delete links for data the user can't modify.</span></span> <span data-ttu-id="79d97-244">A interface do usuário é fixo, aplicando o manipulador de autorização para os modos de exibição.</span><span class="sxs-lookup"><span data-stu-id="79d97-244">The UI is fixed by applying the authorization handler to the views.</span></span>

<span data-ttu-id="79d97-245">Injetar o serviço de autorização no arquivo *Views/_ViewImports.cshtml* para que ele esteja disponível para todos os modos de exibição:</span><span class="sxs-lookup"><span data-stu-id="79d97-245">Inject the authorization service in the *Views/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[](secure-data/samples/final2/Pages/_ViewImports.cshtml?highlight=6-9)]

<span data-ttu-id="79d97-246">A marcação anterior adiciona várias `using` instruções.</span><span class="sxs-lookup"><span data-stu-id="79d97-246">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="79d97-247">Atualização de **editar** e **excluir** links em *Pages/Contacts/Index.cshtml* para elas estão renderizadas somente para usuários com as permissões apropriadas:</span><span class="sxs-lookup"><span data-stu-id="79d97-247">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[](secure-data/samples/final2/Pages/Contacts/Index.cshtml?highlight=34-36,64-999)]

> [!WARNING]
> <span data-ttu-id="79d97-248">Ocultar links de usuários que não tem permissão para alterar os dados, o aplicativo não seguro.</span><span class="sxs-lookup"><span data-stu-id="79d97-248">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="79d97-249">Ocultar links torna o aplicativo mais amigável exibindo links só é válidas.</span><span class="sxs-lookup"><span data-stu-id="79d97-249">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="79d97-250">Os usuários podem hack as URLs geradas para chamar editar e excluir operações nos dados que não possuem.</span><span class="sxs-lookup"><span data-stu-id="79d97-250">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="79d97-251">O Razor de página ou o controlador deve impor verificações de acesso para proteger os dados.</span><span class="sxs-lookup"><span data-stu-id="79d97-251">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="79d97-252">Detalhes da atualização</span><span class="sxs-lookup"><span data-stu-id="79d97-252">Update Details</span></span>

<span data-ttu-id="79d97-253">Atualize a exibição de detalhes para que os gerentes possam aprovar ou rejeitar contatos:</span><span class="sxs-lookup"><span data-stu-id="79d97-253">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[](secure-data/samples/final2/Pages/Contacts/Details.cshtml?range=48-999)]

<span data-ttu-id="79d97-254">Atualize o modelo de página de detalhes:</span><span class="sxs-lookup"><span data-stu-id="79d97-254">Update the details page model:</span></span>

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="test-the-completed-app"></a><span data-ttu-id="79d97-255">Testar o aplicativo concluído</span><span class="sxs-lookup"><span data-stu-id="79d97-255">Test the completed app</span></span>

<span data-ttu-id="79d97-256">Se você estiver usando o Visual Studio Code ou testando em uma plataforma local que não inclui um certificado de teste para HTTPS:</span><span class="sxs-lookup"><span data-stu-id="79d97-256">If you're using Visual Studio Code or testing on a local platform that doesn't include a test certificate for HTTPS:</span></span>

* <span data-ttu-id="79d97-257">Defina `"LocalTest:skipHTTPS": true` no arquivo *appsettings. Developement.JSON*.</span><span class="sxs-lookup"><span data-stu-id="79d97-257">Set `"LocalTest:skipHTTPS": true` in the *appsettings.Developement.json* file to skip the HTTPS requirement.</span></span> <span data-ttu-id="79d97-258">Skip HTTPS somente em um computador de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="79d97-258">Skip HTTPS only on a development machine.</span></span>

<span data-ttu-id="79d97-259">Se o aplicativo tiver contatos:</span><span class="sxs-lookup"><span data-stu-id="79d97-259">If the app has contacts:</span></span>

* <span data-ttu-id="79d97-260">Excluir todos os registros da tabela `Contact` .</span><span class="sxs-lookup"><span data-stu-id="79d97-260">Delete all the records in the `Contact` table.</span></span>
* <span data-ttu-id="79d97-261">Reinicie o aplicativo para propagar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="79d97-261">Restart the app to seed the database.</span></span>

<span data-ttu-id="79d97-262">Registre um usuário para procurar os contatos.</span><span class="sxs-lookup"><span data-stu-id="79d97-262">Register a user for browsing the contacts.</span></span>

<span data-ttu-id="79d97-263">Uma maneira fácil de testar o aplicativo concluído é iniciar três navegadores diferentes (ou versões de incógnita/InPrivate).</span><span class="sxs-lookup"><span data-stu-id="79d97-263">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate versions).</span></span> <span data-ttu-id="79d97-264">Em um navegador, registrar um novo usuário (por exemplo, `test@example.com`).</span><span class="sxs-lookup"><span data-stu-id="79d97-264">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="79d97-265">Entrar para cada localizador com um usuário diferente.</span><span class="sxs-lookup"><span data-stu-id="79d97-265">Sign in to each browser with a different user.</span></span> <span data-ttu-id="79d97-266">Verifique se as seguintes operações:</span><span class="sxs-lookup"><span data-stu-id="79d97-266">Verify the following operations:</span></span>

* <span data-ttu-id="79d97-267">Os usuários registrados podem exibir todos os dados de contato aprovados.</span><span class="sxs-lookup"><span data-stu-id="79d97-267">Registered users can view all the approved contact data.</span></span>
* <span data-ttu-id="79d97-268">Os usuários registrados podem editar/excluir seus próprios dados.</span><span class="sxs-lookup"><span data-stu-id="79d97-268">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="79d97-269">Os gerentes podem aprovar ou rejeitar contatos.</span><span class="sxs-lookup"><span data-stu-id="79d97-269">Managers can approve or reject contact data.</span></span> <span data-ttu-id="79d97-270">A tela `Details` mostra os botões **aprovar** e **rejeitar**.</span><span class="sxs-lookup"><span data-stu-id="79d97-270">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="79d97-271">Os administradores podem Aprovar/rejeitar e editar/excluir todos os dados.</span><span class="sxs-lookup"><span data-stu-id="79d97-271">Administrators can approve/reject and edit/delete any data.</span></span>

| <span data-ttu-id="79d97-272">User</span><span class="sxs-lookup"><span data-stu-id="79d97-272">User</span></span>| <span data-ttu-id="79d97-273">Opções</span><span class="sxs-lookup"><span data-stu-id="79d97-273">Options</span></span> |
| ------------ | ---------|
| test@example.com | <span data-ttu-id="79d97-274">Pode editar/excluir próprios dados</span><span class="sxs-lookup"><span data-stu-id="79d97-274">Can edit/delete own data</span></span> |
| manager@contoso.com | <span data-ttu-id="79d97-275">Rejeitar/aprovar e editar/excluir podem ter dados</span><span class="sxs-lookup"><span data-stu-id="79d97-275">Can approve/reject and edit/delete own data</span></span> |
| admin@contoso.com | <span data-ttu-id="79d97-276">Pode editar/excluir e Aprovar/rejeitar todos os dados</span><span class="sxs-lookup"><span data-stu-id="79d97-276">Can edit/delete and approve/reject all data</span></span>|

<span data-ttu-id="79d97-277">Crie um contato no navegador do administrador.</span><span class="sxs-lookup"><span data-stu-id="79d97-277">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="79d97-278">Copie a URL para excluir e editar o contato de administrador.</span><span class="sxs-lookup"><span data-stu-id="79d97-278">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="79d97-279">Cole esses links no navegador do usuário de teste para verificar se que o usuário de teste não pode executar essas operações.</span><span class="sxs-lookup"><span data-stu-id="79d97-279">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="79d97-280">Criar o aplicativo de início</span><span class="sxs-lookup"><span data-stu-id="79d97-280">Create the starter app</span></span>

* <span data-ttu-id="79d97-281">Criar um aplicativo de páginas Razor denominado "ContactManager"</span><span class="sxs-lookup"><span data-stu-id="79d97-281">Create a Razor Pages app named "ContactManager"</span></span>

  * <span data-ttu-id="79d97-282">Criar o aplicativo com **contas de usuário individuais**.</span><span class="sxs-lookup"><span data-stu-id="79d97-282">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="79d97-283">Coloque o nome de "ContactManager" para que o namespace coincida com o namespace usado no exemplo.</span><span class="sxs-lookup"><span data-stu-id="79d97-283">Name it "ContactManager" so your namespace matches the namespace used in the sample.</span></span>

::: moniker range=">= aspnetcore-2.1"

  ```console
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

  [!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

  ```console
  dotnet new razor -o ContactManager -au Individual -uld
  ```

::: moniker-end

  * <span data-ttu-id="79d97-285">`-uld` Especifica o LocalDB, em vez de SQLite</span><span class="sxs-lookup"><span data-stu-id="79d97-285">`-uld` specifies LocalDB instead of SQLite</span></span>

* <span data-ttu-id="79d97-286">Adicionar o seguinte modelo `Contact` :</span><span class="sxs-lookup"><span data-stu-id="79d97-286">Add the following `Contact` model:</span></span>

  [!code-csharp[](secure-data/samples/starter2/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="79d97-287">Fazer Scaffold do modelo `Contact`:</span><span class="sxs-lookup"><span data-stu-id="79d97-287">Scaffold the `Contact` model:</span></span>

```console
dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
```

* <span data-ttu-id="79d97-288">Atualizar o link de **ContactManager** no arquivo *Pages/_Layout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="79d97-288">Update the **ContactManager** anchor in the *Pages/_Layout.cshtml* file:</span></span>

```cshtml
<a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
```

* <span data-ttu-id="79d97-289">Fazer Scaffold da migração inicial e atualizar o banco de dados:</span><span class="sxs-lookup"><span data-stu-id="79d97-289">Scaffold the initial migration and update the database:</span></span>

```console
dotnet ef migrations add initial
dotnet ef database update
```

* <span data-ttu-id="79d97-290">Testar o aplicativo de criação, edição e exclusão de um contato</span><span class="sxs-lookup"><span data-stu-id="79d97-290">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="79d97-291">Propagar o banco de dados</span><span class="sxs-lookup"><span data-stu-id="79d97-291">Seed the database</span></span>

<span data-ttu-id="79d97-292">Adicionar o `SeedData` de classe para o *dados* pasta.</span><span class="sxs-lookup"><span data-stu-id="79d97-292">Add the `SeedData` class to the *Data* folder.</span></span> <span data-ttu-id="79d97-293">Se você baixou o exemplo, você pode copiar o *SeedData.cs* o arquivo para o *dados* pasta do projeto starter.</span><span class="sxs-lookup"><span data-stu-id="79d97-293">If you've downloaded the sample, you can copy the *SeedData.cs* file to the *Data* folder of the starter project.</span></span>

<span data-ttu-id="79d97-294">Chamar `SeedData.Initialize` de `Main`:</span><span class="sxs-lookup"><span data-stu-id="79d97-294">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[](secure-data/samples/starter2/Program.cs?name=snippet)]

<span data-ttu-id="79d97-295">Teste o aplicativo propagado o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="79d97-295">Test that the app seeded the database.</span></span> <span data-ttu-id="79d97-296">Se houver linhas no banco de dados de contato, o método de propagação não é executado.</span><span class="sxs-lookup"><span data-stu-id="79d97-296">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a><span data-ttu-id="79d97-297">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="79d97-297">Additional resources</span></span>

* <span data-ttu-id="79d97-298">[Laboratório de autorização de ASP.NET Core](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span><span class="sxs-lookup"><span data-stu-id="79d97-298">[ASP.NET Core Authorization Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span></span> <span data-ttu-id="79d97-299">Este laboratório apresenta mais detalhes sobre os recursos de segurança introduzidos neste tutorial.</span><span class="sxs-lookup"><span data-stu-id="79d97-299">This lab goes into more detail on the security features introduced in this tutorial.</span></span>
* [<span data-ttu-id="79d97-300">Autorização no ASP.NET Core: Simple, função, baseada em declarações e personalizada</span><span class="sxs-lookup"><span data-stu-id="79d97-300">Authorization in ASP.NET Core: Simple, role, claims-based, and custom</span></span>](xref:security/authorization/index)
* [<span data-ttu-id="79d97-301">Autorização baseada em política personalizada</span><span class="sxs-lookup"><span data-stu-id="79d97-301">Custom policy-based authorization</span></span>](xref:security/authorization/policies)
