---
title: "Criar um aplicativo do ASP.NET Core com dados de usuário protegidos por autorização"
author: rick-anderson
ms.author: riande
manager: wpickett
ms.date: 05/22/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: security/authorization/secure-data
ms.openlocfilehash: 861ac619c7f5fb19a56c59536e20724d96bbddca
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/19/2018
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a><span data-ttu-id="32910-102">Criar um aplicativo do ASP.NET Core com dados de usuário protegidos por autorização</span><span class="sxs-lookup"><span data-stu-id="32910-102">Create an ASP.NET Core app with user data protected by authorization</span></span>

<span data-ttu-id="32910-103">Por [Rick Anderson](https://twitter.com/RickAndMSFT) e [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="32910-103">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="32910-104">Este tutorial mostra como criar um aplicativo web com dados de usuário protegidos por autorização.</span><span class="sxs-lookup"><span data-stu-id="32910-104">This tutorial shows how to create a web app with user data protected by authorization.</span></span> <span data-ttu-id="32910-105">Exibe uma lista de contatos (registrados) usuários autenticados criou.</span><span class="sxs-lookup"><span data-stu-id="32910-105">It  displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="32910-106">Há três grupos de segurança:</span><span class="sxs-lookup"><span data-stu-id="32910-106">There are three security groups:</span></span>

* <span data-ttu-id="32910-107">Os usuários registrados podem exibir todos os dados de contato aprovados.</span><span class="sxs-lookup"><span data-stu-id="32910-107">Registered users can view all the approved contact data.</span></span>
* <span data-ttu-id="32910-108">Os usuários registrados podem editar/excluir seus próprios dados.</span><span class="sxs-lookup"><span data-stu-id="32910-108">Registered users can edit/delete their own data.</span></span> 
* <span data-ttu-id="32910-109">Os gerentes podem aprovar ou rejeitar dados de contato.</span><span class="sxs-lookup"><span data-stu-id="32910-109">Managers can approve or reject contact data.</span></span> <span data-ttu-id="32910-110">Apenas os contatos aprovados são visíveis aos usuários.</span><span class="sxs-lookup"><span data-stu-id="32910-110">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="32910-111">Os administradores podem Aprovar/rejeitar e editar/excluir todos os dados.</span><span class="sxs-lookup"><span data-stu-id="32910-111">Administrators can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="32910-112">Na imagem a seguir, o usuário Rick (`rick@example.com`) está conectado.</span><span class="sxs-lookup"><span data-stu-id="32910-112">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="32910-113">Usuário Rick pode apenas exibir aprovado contatos e editar/excluir seus contatos.</span><span class="sxs-lookup"><span data-stu-id="32910-113">User Rick can only view approved contacts and edit/delete his contacts.</span></span> <span data-ttu-id="32910-114">Somente o último registro criado pelo Rick, exibe editar e excluir links</span><span class="sxs-lookup"><span data-stu-id="32910-114">Only the last record, created by Rick, displays edit and delete links</span></span>

![imagem descrita acima](secure-data/_static/rick.png)

<span data-ttu-id="32910-116">Na imagem a seguir, `manager@contoso.com` está conectado e na função de gerentes.</span><span class="sxs-lookup"><span data-stu-id="32910-116">In the following image, `manager@contoso.com` is signed in and in the managers role.</span></span> 

![imagem descrita acima](secure-data/_static/manager1.png)

<span data-ttu-id="32910-118">A imagem a seguir mostra os gerentes de modo de exibição de detalhes de um contato.</span><span class="sxs-lookup"><span data-stu-id="32910-118">The following image shows the  managers details view of a contact.</span></span>

![imagem descrita acima](secure-data/_static/manager.png)

<span data-ttu-id="32910-120">Somente os gerentes e os administradores têm a aprovar e rejeitar botões.</span><span class="sxs-lookup"><span data-stu-id="32910-120">Only managers and administrators have the approve and reject buttons.</span></span>

<span data-ttu-id="32910-121">Na imagem a seguir, `admin@contoso.com` está conectado e na função do administrador.</span><span class="sxs-lookup"><span data-stu-id="32910-121">In the following image, `admin@contoso.com` is signed in and in the administrator’s role.</span></span> 

![imagem descrita acima](secure-data/_static/admin.png)

<span data-ttu-id="32910-123">O administrador tem todos os privilégios.</span><span class="sxs-lookup"><span data-stu-id="32910-123">The administrator has all privileges.</span></span> <span data-ttu-id="32910-124">Ela pode ler/editar/excluir qualquer contato e alterar o status de contatos.</span><span class="sxs-lookup"><span data-stu-id="32910-124">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="32910-125">O aplicativo foi criado por [scaffolding](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) o seguinte `Contact` modelo:</span><span class="sxs-lookup"><span data-stu-id="32910-125">The app was created by [scaffolding](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller)  the following `Contact` model:</span></span>

[!code-csharp[Main](secure-data/samples/starter/Models/Contact.cs?name=snippet1)]

<span data-ttu-id="32910-126">Um `ContactIsOwnerAuthorizationHandler` manipulador de autorização garante que um usuário só pode editar seus dados.</span><span class="sxs-lookup"><span data-stu-id="32910-126">A `ContactIsOwnerAuthorizationHandler` authorization handler ensures that a user can only edit their data.</span></span> <span data-ttu-id="32910-127">Um `ContactManagerAuthorizationHandler` manipulador de autorização permite que os gerentes aprovar ou rejeitar contatos.</span><span class="sxs-lookup"><span data-stu-id="32910-127">A `ContactManagerAuthorizationHandler` authorization handler allows managers to approve or reject contacts.</span></span>  <span data-ttu-id="32910-128">Um `ContactAdministratorsAuthorizationHandler` manipulador de autorização permite que os administradores para aprovar ou rejeitar contatos e editar/excluir contatos.</span><span class="sxs-lookup"><span data-stu-id="32910-128">A `ContactAdministratorsAuthorizationHandler` authorization handler allows administrators to approve or reject contacts and to edit/delete contacts.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="32910-129">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="32910-129">Prerequisites</span></span>

<span data-ttu-id="32910-130">Este não é um tutorial de início.</span><span class="sxs-lookup"><span data-stu-id="32910-130">This is not a beginning tutorial.</span></span> <span data-ttu-id="32910-131">Você deve estar familiarizado com:</span><span class="sxs-lookup"><span data-stu-id="32910-131">You should be familiar with:</span></span>

* [<span data-ttu-id="32910-132">ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="32910-132">ASP.NET Core MVC</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="32910-133">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="32910-133">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="32910-134">O início e o aplicativo concluído</span><span class="sxs-lookup"><span data-stu-id="32910-134">The starter and completed app</span></span>

<span data-ttu-id="32910-135">[Baixar](xref:tutorials/index#how-to-download-a-sample) o [concluída](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final) aplicativo.</span><span class="sxs-lookup"><span data-stu-id="32910-135">[Download](xref:tutorials/index#how-to-download-a-sample) the [completed](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final) app.</span></span> <span data-ttu-id="32910-136">[Teste](#test-the-completed-app) o aplicativo concluído para que você se familiarize com seus recursos de segurança.</span><span class="sxs-lookup"><span data-stu-id="32910-136">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span> 

### <a name="the-starter-app"></a><span data-ttu-id="32910-137">O aplicativo de início</span><span class="sxs-lookup"><span data-stu-id="32910-137">The starter app</span></span>

<span data-ttu-id="32910-138">É útil comparar seu código com o exemplo concluído.</span><span class="sxs-lookup"><span data-stu-id="32910-138">It's helpful to compare your code with the completed sample.</span></span>

<span data-ttu-id="32910-139">[Baixar](xref:tutorials/index#how-to-download-a-sample) o [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter) aplicativo.</span><span class="sxs-lookup"><span data-stu-id="32910-139">[Download](xref:tutorials/index#how-to-download-a-sample) the [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter) app.</span></span> 

<span data-ttu-id="32910-140">Consulte [criar o aplicativo de início](#create-the-starter-app) se deseja criá-lo a partir do zero.</span><span class="sxs-lookup"><span data-stu-id="32910-140">See [Create the starter app](#create-the-starter-app) if you'd like to create it from scratch.</span></span>

<span data-ttu-id="32910-141">Atualize o banco de dados:</span><span class="sxs-lookup"><span data-stu-id="32910-141">Update the database:</span></span>

```none
   dotnet ef database update
```

<span data-ttu-id="32910-142">Executar o aplicativo, toque o **ContactManager** vincular e verifique se você pode criar, editar e excluir um contato.</span><span class="sxs-lookup"><span data-stu-id="32910-142">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

<span data-ttu-id="32910-143">Este tutorial tem todas as etapas principais para criar o aplicativo de dados de usuário segura.</span><span class="sxs-lookup"><span data-stu-id="32910-143">This tutorial has all the major steps to create the secure user data app.</span></span> <span data-ttu-id="32910-144">Talvez seja útil para fazer referência ao projeto concluído.</span><span class="sxs-lookup"><span data-stu-id="32910-144">You may find it helpful to refer to the completed project.</span></span>

## <a name="modify-the-app-to-secure-user-data"></a><span data-ttu-id="32910-145">Modificar o aplicativo para proteger os dados de usuário</span><span class="sxs-lookup"><span data-stu-id="32910-145">Modify the app to secure user data</span></span>

<span data-ttu-id="32910-146">As seções a seguir têm todas as principais etapas para criar o aplicativo de dados de usuário segura.</span><span class="sxs-lookup"><span data-stu-id="32910-146">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="32910-147">Talvez seja útil para fazer referência ao projeto concluído.</span><span class="sxs-lookup"><span data-stu-id="32910-147">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="32910-148">Vincular os dados de contato para o usuário</span><span class="sxs-lookup"><span data-stu-id="32910-148">Tie the contact data to the user</span></span>

<span data-ttu-id="32910-149">Usar o ASP.NET [identidade](xref:security/authentication/identity) ID de usuário para garantir que os usuários pode editar seus dados, mas não de outros dados de usuários.</span><span class="sxs-lookup"><span data-stu-id="32910-149">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="32910-150">Adicionar `OwnerID` para o `Contact` modelo:</span><span class="sxs-lookup"><span data-stu-id="32910-150">Add `OwnerID` to the `Contact` model:</span></span>

[!code-csharp[Main](secure-data/samples/final/Models/Contact.cs?name=snippet1&highlight=5-6,16-)]

<span data-ttu-id="32910-151">`OwnerID`é a ID do usuário da `AspNetUser` tabela o [identidade](xref:security/authentication/identity) banco de dados.</span><span class="sxs-lookup"><span data-stu-id="32910-151">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="32910-152">O `Status` campo determina se um contato é exibido por usuários gerais.</span><span class="sxs-lookup"><span data-stu-id="32910-152">The `Status` field determines if a contact is viewable by general users.</span></span> 

<span data-ttu-id="32910-153">Scaffold uma nova migração e atualizar o banco de dados:</span><span class="sxs-lookup"><span data-stu-id="32910-153">Scaffold a new migration and update the database:</span></span>

```console
dotnet ef migrations add userID_Status
dotnet ef database update
 ```

### <a name="require-ssl-and-authenticated-users"></a><span data-ttu-id="32910-154">Exigir SSL e os usuários autenticados</span><span class="sxs-lookup"><span data-stu-id="32910-154">Require SSL and authenticated users</span></span>

<span data-ttu-id="32910-155">No `ConfigureServices` método o *Startup.cs* de arquivo, adicione o [RequireHttpsAttribute](/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) filtro de autorização:</span><span class="sxs-lookup"><span data-stu-id="32910-155">In the `ConfigureServices` method of the *Startup.cs* file, add the [RequireHttpsAttribute](/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) authorization filter:</span></span>

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=snippet_SSL&highlight=1)]

<span data-ttu-id="32910-156">Para redirecionar solicitações HTTP para HTTPS, consulte [Middleware de regravação de URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="32910-156">To redirect HTTP requests to HTTPS, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="32910-157">Se você estiver usando o código do Visual Studio ou teste na plataforma local que não inclui um certificado de teste para SSL:</span><span class="sxs-lookup"><span data-stu-id="32910-157">If you are using Visual Studio Code or testing on local platform that doesn't include a test certificate for SSL:</span></span>

- <span data-ttu-id="32910-158">Definir `"LocalTest:skipSSL": true` no *appSettings. JSON* arquivo.</span><span class="sxs-lookup"><span data-stu-id="32910-158">Set `"LocalTest:skipSSL": true` in the *appsettings.json* file.</span></span>

### <a name="require-authenticated-users"></a><span data-ttu-id="32910-159">Exigir que os usuários autenticados</span><span class="sxs-lookup"><span data-stu-id="32910-159">Require authenticated users</span></span>

<span data-ttu-id="32910-160">Defina a política de autenticação padrão para exigir que os usuários sejam autenticados.</span><span class="sxs-lookup"><span data-stu-id="32910-160">Set the default authentication policy to require users to be authenticated.</span></span> <span data-ttu-id="32910-161">Você pode recusar a autenticação, o método de ação ou controlador com o `[AllowAnonymous]` atributo.</span><span class="sxs-lookup"><span data-stu-id="32910-161">You can opt out of authentication at the controller or action method with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="32910-162">Com essa abordagem, quaisquer novos controladores adicionados automaticamente exigir autenticação, que é mais segura do que contar com novos controladores para incluir o `[Authorize]` atributo.</span><span class="sxs-lookup"><span data-stu-id="32910-162">With this approach, any new controllers added will automatically require authentication, which is safer than relying on new controllers to include the `[Authorize]` attribute.</span></span> <span data-ttu-id="32910-163">Adicione o seguinte para o `ConfigureServices` método o *Startup.cs* arquivo:</span><span class="sxs-lookup"><span data-stu-id="32910-163">Add the following to  the `ConfigureServices` method of the *Startup.cs* file:</span></span>

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=snippet_defaultPolicy)]

<span data-ttu-id="32910-164">Adicionar `[AllowAnonymous]` para o controlador principal para usuários anônimos podem obter informações sobre o site antes de eles se registrar.</span><span class="sxs-lookup"><span data-stu-id="32910-164">Add `[AllowAnonymous]` to the home controller so anonymous users can get information about the site before they register.</span></span>

[!code-csharp[Main](secure-data/samples/final/Controllers/HomeController.cs?name=snippet1&highlight=2,6)]

### <a name="configure-the-test-account"></a><span data-ttu-id="32910-165">Configurar a conta de teste</span><span class="sxs-lookup"><span data-stu-id="32910-165">Configure the test account</span></span>

<span data-ttu-id="32910-166">O `SeedData` classe cria duas contas de administrador e Gerenciador.</span><span class="sxs-lookup"><span data-stu-id="32910-166">The `SeedData` class creates two accounts,  administrator and manager.</span></span> <span data-ttu-id="32910-167">Use o [ferramenta Gerenciador de segredo](xref:security/app-secrets) para definir uma senha para essas contas.</span><span class="sxs-lookup"><span data-stu-id="32910-167">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="32910-168">Fazer isso no diretório do projeto (o diretório que contém *Program.cs*).</span><span class="sxs-lookup"><span data-stu-id="32910-168">Do this from the project directory (the directory containing *Program.cs*).</span></span>

```console
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="32910-169">Atualização `Configure` para usar a senha de teste:</span><span class="sxs-lookup"><span data-stu-id="32910-169">Update `Configure` to use the test password:</span></span>

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=Configure&highlight=19-21)]

<span data-ttu-id="32910-170">Adicione a ID de usuário de administrador e `Status = ContactStatus.Approved` para os contatos.</span><span class="sxs-lookup"><span data-stu-id="32910-170">Add the administrator user ID and `Status = ContactStatus.Approved` to the contacts.</span></span> <span data-ttu-id="32910-171">Somente um contato é exibido, adicione a ID de usuário para todos os contatos:</span><span class="sxs-lookup"><span data-stu-id="32910-171">Only one contact is shown, add the user ID to all contacts:</span></span>

[!code-csharp[Main](secure-data/samples/final/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="32910-172">Criar proprietário, Gerenciador e manipuladores de autorização do administrador</span><span class="sxs-lookup"><span data-stu-id="32910-172">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="32910-173">Criar um `ContactIsOwnerAuthorizationHandler` classe no *autorização* pasta.</span><span class="sxs-lookup"><span data-stu-id="32910-173">Create a `ContactIsOwnerAuthorizationHandler` class in the  *Authorization* folder.</span></span> <span data-ttu-id="32910-174">O `ContactIsOwnerAuthorizationHandler` verificará se o usuário atuando no recurso possui o recurso.</span><span class="sxs-lookup"><span data-stu-id="32910-174">The `ContactIsOwnerAuthorizationHandler` will verify the user acting on the resource owns the resource.</span></span>

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="32910-175">O `ContactIsOwnerAuthorizationHandler` chamadas `context.Succeed` se o usuário autenticado atual é o proprietário do contato.</span><span class="sxs-lookup"><span data-stu-id="32910-175">The `ContactIsOwnerAuthorizationHandler` calls `context.Succeed` if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="32910-176">Manipuladores de autorização geralmente retornar `context.Succeed` quando os requisitos são atendidos.</span><span class="sxs-lookup"><span data-stu-id="32910-176">Authorization handlers generally return `context.Succeed` when the requirements are met.</span></span> <span data-ttu-id="32910-177">Elas retornam `Task.FromResult(0)` quando os requisitos não forem atendidos.</span><span class="sxs-lookup"><span data-stu-id="32910-177">They return `Task.FromResult(0)` when requirements are not met.</span></span> <span data-ttu-id="32910-178">`Task.FromResult(0)`não é êxito ou falha, ele permite que outro manipulador de autorização executar.</span><span class="sxs-lookup"><span data-stu-id="32910-178">`Task.FromResult(0)` is neither success or failure, it allows other authorization handler to run.</span></span> <span data-ttu-id="32910-179">Se você precisar explicitamente falhar, retorne `context.Fail()`.</span><span class="sxs-lookup"><span data-stu-id="32910-179">If you need to explicitly fail, return `context.Fail()`.</span></span>

<span data-ttu-id="32910-180">Permitir que proprietários de contato editar/excluir seus próprios dados, portanto, não precisamos verificar a operação passada no parâmetro de requisito.</span><span class="sxs-lookup"><span data-stu-id="32910-180">We allow contact owners to edit/delete their own data, so we don't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="32910-181">Criar um manipulador de autorização do Gerenciador</span><span class="sxs-lookup"><span data-stu-id="32910-181">Create a manager authorization handler</span></span>

<span data-ttu-id="32910-182">Criar um `ContactManagerAuthorizationHandler` classe no *autorização* pasta.</span><span class="sxs-lookup"><span data-stu-id="32910-182">Create a `ContactManagerAuthorizationHandler` class in the  *Authorization* folder.</span></span> <span data-ttu-id="32910-183">O `ContactManagerAuthorizationHandler` verificará se o usuário atuando no recurso é um gerenciador.</span><span class="sxs-lookup"><span data-stu-id="32910-183">The `ContactManagerAuthorizationHandler` will verify the user acting on the resource is a manager.</span></span> <span data-ttu-id="32910-184">Somente os gerentes podem aprovar ou rejeitar alterações de conteúdo (nova ou alteradas).</span><span class="sxs-lookup"><span data-stu-id="32910-184">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="32910-185">Criar um manipulador de autorização do administrador</span><span class="sxs-lookup"><span data-stu-id="32910-185">Create an administrator authorization handler</span></span>

<span data-ttu-id="32910-186">Criar um `ContactAdministratorsAuthorizationHandler` classe no *autorização* pasta.</span><span class="sxs-lookup"><span data-stu-id="32910-186">Create a `ContactAdministratorsAuthorizationHandler` class in the  *Authorization* folder.</span></span> <span data-ttu-id="32910-187">O `ContactAdministratorsAuthorizationHandler` verificará se o usuário atuando no recurso é um administrador.</span><span class="sxs-lookup"><span data-stu-id="32910-187">The `ContactAdministratorsAuthorizationHandler` will verify the user acting on the resource is a administrator.</span></span> <span data-ttu-id="32910-188">Administrador pode fazer todas as operações.</span><span class="sxs-lookup"><span data-stu-id="32910-188">Administrator can do all operations.</span></span>

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="32910-189">Registrar os manipuladores de autorização</span><span class="sxs-lookup"><span data-stu-id="32910-189">Register the authorization handlers</span></span>

<span data-ttu-id="32910-190">Serviços usando o Entity Framework Core devem ser registrados para [injeção de dependência](xref:fundamentals/dependency-injection) usando [AddScoped](/aspnet/core/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span><span class="sxs-lookup"><span data-stu-id="32910-190">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/aspnet/core/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="32910-191">O `ContactIsOwnerAuthorizationHandler` usa o ASP.NET Core [identidade](xref:security/authentication/identity), que está incluído no Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="32910-191">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="32910-192">Registrar os manipuladores com a coleção de serviço para que estejam disponíveis para o `ContactsController` por meio de [injeção de dependência](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="32910-192">Register the handlers with the service collection so they will be available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="32910-193">Adicione o seguinte código ao final da `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="32910-193">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=AuthorizationHandlers)]

<span data-ttu-id="32910-194">`ContactAdministratorsAuthorizationHandler`e `ContactManagerAuthorizationHandler` são adicionados como singletons.</span><span class="sxs-lookup"><span data-stu-id="32910-194">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="32910-195">São singletons porque eles não usam o EF e todas as informações necessárias no `Context` parâmetro o `HandleRequirementAsync` método.</span><span class="sxs-lookup"><span data-stu-id="32910-195">They are singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

<span data-ttu-id="32910-196">Completo `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="32910-196">The complete `ConfigureServices`:</span></span>

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=ConfigureServices)]

## <a name="update-the-code-to-support-authorization"></a><span data-ttu-id="32910-197">Atualizar o código para oferecer suporte à autorização</span><span class="sxs-lookup"><span data-stu-id="32910-197">Update the code to support authorization</span></span>

<span data-ttu-id="32910-198">Nesta seção, você atualizar o controlador e os modos de exibição e adicionar uma classe de requisitos de operações.</span><span class="sxs-lookup"><span data-stu-id="32910-198">In this section, you update the controller and views and add an operations requirements class.</span></span>

### <a name="update-the-contacts-controller"></a><span data-ttu-id="32910-199">Atualize o controlador de contatos</span><span class="sxs-lookup"><span data-stu-id="32910-199">Update the Contacts controller</span></span>

<span data-ttu-id="32910-200">Atualização de `ContactsController` construtor:</span><span class="sxs-lookup"><span data-stu-id="32910-200">Update the `ContactsController` constructor:</span></span>

* <span data-ttu-id="32910-201">Adicionar o `IAuthorizationService` serviço acesse os manipuladores de autorização.</span><span class="sxs-lookup"><span data-stu-id="32910-201">Add the `IAuthorizationService` service to  access to the authorization handlers.</span></span> 
* <span data-ttu-id="32910-202">Adicionar o `Identity` `UserManager` serviço:</span><span class="sxs-lookup"><span data-stu-id="32910-202">Add the `Identity` `UserManager` service:</span></span>

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_ContactsControllerCtor)]

### <a name="add-a-contact-operations-requirements-class"></a><span data-ttu-id="32910-203">Adicionar uma classe de requisitos de operações de contato</span><span class="sxs-lookup"><span data-stu-id="32910-203">Add a contact operations requirements class</span></span>

<span data-ttu-id="32910-204">Adicionar o `ContactOperations` de classe para o *autorização* pasta.</span><span class="sxs-lookup"><span data-stu-id="32910-204">Add the `ContactOperations` class to the *Authorization* folder.</span></span> <span data-ttu-id="32910-205">Essa classe contém os requisitos de nosso aplicativo suporta:</span><span class="sxs-lookup"><span data-stu-id="32910-205">This class  contain the requirements our app supports:</span></span>

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactOperations.cs)]

### <a name="update-create"></a><span data-ttu-id="32910-206">Criar atualização</span><span class="sxs-lookup"><span data-stu-id="32910-206">Update Create</span></span>

<span data-ttu-id="32910-207">Atualização de `HTTP POST Create` método:</span><span class="sxs-lookup"><span data-stu-id="32910-207">Update the `HTTP POST Create` method to:</span></span>

* <span data-ttu-id="32910-208">Adicione a ID de usuário para o `Contact` modelo.</span><span class="sxs-lookup"><span data-stu-id="32910-208">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="32910-209">Chama o manipulador de autorização para verificar se que o usuário possui o contato.</span><span class="sxs-lookup"><span data-stu-id="32910-209">Call the authorization handler to verify the user owns the contact.</span></span>

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Create)]

### <a name="update-edit"></a><span data-ttu-id="32910-210">Atualização de edição</span><span class="sxs-lookup"><span data-stu-id="32910-210">Update Edit</span></span>

<span data-ttu-id="32910-211">Atualizar `Edit` métodos para usar o manipulador de autorização para verificar se o usuário possui o contato.</span><span class="sxs-lookup"><span data-stu-id="32910-211">Update both `Edit` methods to use the authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="32910-212">Como estamos executando a autorização de recursos não podemos usar o `[Authorize]` atributo.</span><span class="sxs-lookup"><span data-stu-id="32910-212">Because we are performing resource authorization we cannot use the `[Authorize]` attribute.</span></span> <span data-ttu-id="32910-213">Não temos acesso ao recurso quando atributos são avaliados.</span><span class="sxs-lookup"><span data-stu-id="32910-213">We don't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="32910-214">Autorização de recursos com base deve ser obrigatória.</span><span class="sxs-lookup"><span data-stu-id="32910-214">Resource based authorization must be imperative.</span></span> <span data-ttu-id="32910-215">Verificações de devem ser executadas depois que temos acesso ao recurso, carregando-o em nosso controlador ou carregá-lo dentro do manipulador de si mesmo.</span><span class="sxs-lookup"><span data-stu-id="32910-215">Checks must be performed once we have access to the resource, either by loading it in our controller, or by loading it within the handler itself.</span></span> <span data-ttu-id="32910-216">Frequentemente, você vai acessar o recurso, passando a chave de recurso.</span><span class="sxs-lookup"><span data-stu-id="32910-216">Frequently you will access the resource by passing in the resource key.</span></span>

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Edit)]

### <a name="update-the-delete-method"></a><span data-ttu-id="32910-217">Atualizar o método Delete</span><span class="sxs-lookup"><span data-stu-id="32910-217">Update the Delete method</span></span>

<span data-ttu-id="32910-218">Atualizar `Delete` métodos para usar o manipulador de autorização para verificar se o usuário possui o contato.</span><span class="sxs-lookup"><span data-stu-id="32910-218">Update both `Delete` methods to use the authorization handler to verify the user owns the contact.</span></span>

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Delete)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="32910-219">Injetar o serviço de autorização para os modos de exibição</span><span class="sxs-lookup"><span data-stu-id="32910-219">Inject the authorization service into the views</span></span>

<span data-ttu-id="32910-220">Atualmente a interface do usuário mostra editar e excluir links de dados que o usuário não pode modificar.</span><span class="sxs-lookup"><span data-stu-id="32910-220">Currently the UI shows edit and delete links for data the user cannot modify.</span></span> <span data-ttu-id="32910-221">Corrigiremos que aplicando o manipulador de autorização para os modos de exibição.</span><span class="sxs-lookup"><span data-stu-id="32910-221">We'll fix that by applying the authorization handler to the views.</span></span>

<span data-ttu-id="32910-222">Injetar o serviço de autorização no *Views/_ViewImports.cshtml* arquivo para que fique disponível para todos os modos de exibição:</span><span class="sxs-lookup"><span data-stu-id="32910-222">Inject the authorization service in the *Views/_ViewImports.cshtml* file so it will be available to all views:</span></span>

[!code-html[Main](secure-data/samples/final/Views/_ViewImports.cshtml)]

<span data-ttu-id="32910-223">Atualização de *Views/Contacts/Index.cshtml* exibição do Razor para somente exibir editar e excluir links para os usuários que podem editar/excluir o contato.</span><span class="sxs-lookup"><span data-stu-id="32910-223">Update the *Views/Contacts/Index.cshtml* Razor view to only display the edit and delete links for users who can edit/delete the contact.</span></span>

<span data-ttu-id="32910-224">Adicionar`@using ContactManager.Authorization;`</span><span class="sxs-lookup"><span data-stu-id="32910-224">Add `@using ContactManager.Authorization;`</span></span>

<span data-ttu-id="32910-225">Atualização de `Edit` e `Delete` links para elas são renderizadas somente para usuários com permissão Editar e excluir o contato.</span><span class="sxs-lookup"><span data-stu-id="32910-225">Update the `Edit` and `Delete` links so they are only rendered for users with permission to edit and delete the contact.</span></span>

[!code-html[Main](secure-data/samples/final/Views/Contacts/Index.cshtml?range=63-84)]

<span data-ttu-id="32910-226">Aviso: Ocultar links de usuários que não têm permissão para editar ou excluir dados não protege o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="32910-226">Warning: Hiding links from users that do not have permission to edit or delete data does not secure the app.</span></span> <span data-ttu-id="32910-227">Ocultar links torna o aplicativo de usuário mais amigável exibindo links só é válidas.</span><span class="sxs-lookup"><span data-stu-id="32910-227">Hiding links makes the app more user friendly by displaying only valid links.</span></span> <span data-ttu-id="32910-228">Os usuários podem hack as URLs geradas para chamar editar e excluir operações nos dados que não possuem.</span><span class="sxs-lookup"><span data-stu-id="32910-228">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span>  <span data-ttu-id="32910-229">O controlador deve repetir que verifica o acesso para ser protegido.</span><span class="sxs-lookup"><span data-stu-id="32910-229">The controller must repeat the access checks to be secure.</span></span>

### <a name="update-the-details-view"></a><span data-ttu-id="32910-230">Atualizar a exibição de detalhes</span><span class="sxs-lookup"><span data-stu-id="32910-230">Update the Details view</span></span>

<span data-ttu-id="32910-231">Atualize a exibição de detalhes para que os gerentes podem aprovar ou rejeitar contatos:</span><span class="sxs-lookup"><span data-stu-id="32910-231">Update the details view so managers can approve or reject contacts:</span></span>

[!code-html[Main](secure-data/samples/final/Views/Contacts/Details.cshtml?range=53-)]

## <a name="test-the-completed-app"></a><span data-ttu-id="32910-232">Testar o aplicativo concluído</span><span class="sxs-lookup"><span data-stu-id="32910-232">Test the completed app</span></span>

<span data-ttu-id="32910-233">Se você estiver usando o código do Visual Studio ou teste na plataforma local que não inclui um certificado de teste para SSL:</span><span class="sxs-lookup"><span data-stu-id="32910-233">If you are using Visual Studio Code or testing on local platform that doesn't include a test certificate for SSL:</span></span>

- <span data-ttu-id="32910-234">Definir `"LocalTest:skipSSL": true` no *appSettings. JSON* arquivo.</span><span class="sxs-lookup"><span data-stu-id="32910-234">Set `"LocalTest:skipSSL": true` in the *appsettings.json* file.</span></span>

<span data-ttu-id="32910-235">Se você executa o aplicativo e contatos, exclua todos os registros de `Contact` de tabela e reinicie o aplicativo para propagar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="32910-235">If you have run the app and have contacts, delete all the records in the `Contact` table and restart the app to seed the database.</span></span> <span data-ttu-id="32910-236">Se você estiver usando o Visual Studio, você precisa sair e reiniciar o IIS Express para propagar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="32910-236">If you are using Visual Studio, you need to exit and restart IIS Express to seed the database.</span></span>

<span data-ttu-id="32910-237">Registre um usuário para procurar os contatos.</span><span class="sxs-lookup"><span data-stu-id="32910-237">Register a user to browse the contacts.</span></span>

<span data-ttu-id="32910-238">Uma maneira fácil de testar o aplicativo concluído é iniciar três navegadores diferentes (ou versões de incógnita/InPrivate).</span><span class="sxs-lookup"><span data-stu-id="32910-238">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate versions).</span></span> <span data-ttu-id="32910-239">Em um navegador, registrar um novo usuário, por exemplo, `test@example.com`.</span><span class="sxs-lookup"><span data-stu-id="32910-239">In one browser, register a new user, for example, `test@example.com`.</span></span> <span data-ttu-id="32910-240">Entrar para cada localizador com um usuário diferente.</span><span class="sxs-lookup"><span data-stu-id="32910-240">Sign in to each browser with a different user.</span></span> <span data-ttu-id="32910-241">Verifique o seguinte:</span><span class="sxs-lookup"><span data-stu-id="32910-241">Verify the following:</span></span>

* <span data-ttu-id="32910-242">Os usuários registrados podem exibir todos os dados de contato aprovados.</span><span class="sxs-lookup"><span data-stu-id="32910-242">Registered users can view all the approved contact data.</span></span>
* <span data-ttu-id="32910-243">Os usuários registrados podem editar/excluir seus próprios dados.</span><span class="sxs-lookup"><span data-stu-id="32910-243">Registered users can edit/delete their own data.</span></span> 
* <span data-ttu-id="32910-244">Os gerentes podem aprovar ou rejeitar dados de contato.</span><span class="sxs-lookup"><span data-stu-id="32910-244">Managers can approve or reject contact data.</span></span> <span data-ttu-id="32910-245">O `Details` exibição mostra **aprovar** e **rejeitar** botões.</span><span class="sxs-lookup"><span data-stu-id="32910-245">The `Details` view shows **Approve** and **Reject** buttons.</span></span> 
* <span data-ttu-id="32910-246">Os administradores podem Aprovar/rejeitar e editar/excluir todos os dados.</span><span class="sxs-lookup"><span data-stu-id="32910-246">Administrators can approve/reject and edit/delete any data.</span></span>

| <span data-ttu-id="32910-247">User</span><span class="sxs-lookup"><span data-stu-id="32910-247">User</span></span>| <span data-ttu-id="32910-248">Opções</span><span class="sxs-lookup"><span data-stu-id="32910-248">Options</span></span> |
| ------------ | ---------|
| test@example.com | <span data-ttu-id="32910-249">Pode editar/excluir próprios dados</span><span class="sxs-lookup"><span data-stu-id="32910-249">Can edit/delete own data</span></span> |
| manager@contoso.com | <span data-ttu-id="32910-250">Rejeitar/aprovar e editar/excluir podem ter dados</span><span class="sxs-lookup"><span data-stu-id="32910-250">Can approve/reject and edit/delete own data</span></span>  |
| admin@contoso.com | <span data-ttu-id="32910-251">Pode editar/excluir e Aprovar/rejeitar todos os dados</span><span class="sxs-lookup"><span data-stu-id="32910-251">Can edit/delete and approve/reject all data</span></span>|

<span data-ttu-id="32910-252">Crie um contato no navegador de administradores.</span><span class="sxs-lookup"><span data-stu-id="32910-252">Create a contact in the administrators browser.</span></span> <span data-ttu-id="32910-253">Copie a URL para excluir e editar o contato de administrador.</span><span class="sxs-lookup"><span data-stu-id="32910-253">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="32910-254">Cole esses links no navegador do usuário de teste para verificar se que o usuário de teste não pode executar essas operações.</span><span class="sxs-lookup"><span data-stu-id="32910-254">Paste these links into the test user's browser to verify the test user cannot perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="32910-255">Criar o aplicativo de início</span><span class="sxs-lookup"><span data-stu-id="32910-255">Create the starter app</span></span>

<span data-ttu-id="32910-256">Siga estas instruções para criar o aplicativo de início.</span><span class="sxs-lookup"><span data-stu-id="32910-256">Follow these instructions to create the starter app.</span></span>

* <span data-ttu-id="32910-257">Criar um **aplicativo Web do ASP.NET Core** usando [2017 do Visual Studio](https://www.visualstudio.com/) chamado "ContactManager"</span><span class="sxs-lookup"><span data-stu-id="32910-257">Create an **ASP.NET Core Web Application** using [Visual Studio 2017](https://www.visualstudio.com/) named "ContactManager"</span></span>

  * <span data-ttu-id="32910-258">Criar o aplicativo com **contas de usuário individuais**.</span><span class="sxs-lookup"><span data-stu-id="32910-258">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="32910-259">O nome "ContactManager" para que o namespace corresponderá o uso de namespace no exemplo.</span><span class="sxs-lookup"><span data-stu-id="32910-259">Name it "ContactManager" so your namespace will match the namespace use in the sample.</span></span>

* <span data-ttu-id="32910-260">Adicione o seguinte `Contact` modelo:</span><span class="sxs-lookup"><span data-stu-id="32910-260">Add the following `Contact` model:</span></span>

  [!code-csharp[Main](secure-data/samples/starter/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="32910-261">Scaffold o `Contact` modelo usando o Entity Framework Core e o `ApplicationDbContext` contexto de dados.</span><span class="sxs-lookup"><span data-stu-id="32910-261">Scaffold the `Contact` model using Entity Framework Core and the `ApplicationDbContext` data context.</span></span> <span data-ttu-id="32910-262">Aceite todos os padrões de scaffolding.</span><span class="sxs-lookup"><span data-stu-id="32910-262">Accept all the scaffolding defaults.</span></span> <span data-ttu-id="32910-263">Usando `ApplicationDbContext` para o contexto de dados classe coloca a tabela de contato na [identidade](xref:security/authentication/identity) banco de dados.</span><span class="sxs-lookup"><span data-stu-id="32910-263">Using `ApplicationDbContext` for the data context class  puts the contact table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="32910-264">Consulte [adicionando um modelo de](xref:tutorials/first-mvc-app/adding-model) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="32910-264">See [Adding a model](xref:tutorials/first-mvc-app/adding-model) for more information.</span></span>

* <span data-ttu-id="32910-265">Atualização o **ContactManager** fixar no *Views/Shared/_Layout.cshtml* arquivo `asp-controller="Home"` para `asp-controller="Contacts"` tocando assim o **ContactManager** link chamar o controlador de contatos.</span><span class="sxs-lookup"><span data-stu-id="32910-265">Update the **ContactManager** anchor in the *Views/Shared/_Layout.cshtml* file from `asp-controller="Home"` to `asp-controller="Contacts"` so tapping the **ContactManager** link will invoke the Contacts controller.</span></span> <span data-ttu-id="32910-266">A marcação original:</span><span class="sxs-lookup"><span data-stu-id="32910-266">The original markup:</span></span>

```html
   <a asp-area="" asp-controller="Home" asp-action="Index" class="navbar-brand">ContactManager</a>
   ```

<span data-ttu-id="32910-267">A marcação atualizada:</span><span class="sxs-lookup"><span data-stu-id="32910-267">The updated markup:</span></span>

```html
   <a asp-area="" asp-controller="Contacts" asp-action="Index" class="navbar-brand">ContactManager</a>
   ```

* <span data-ttu-id="32910-268">Scaffold a migração inicial e atualizar o banco de dados</span><span class="sxs-lookup"><span data-stu-id="32910-268">Scaffold the initial migration and update the database</span></span>

```none
   dotnet ef migrations add initial
   dotnet ef database update
   ```

* <span data-ttu-id="32910-269">Testar o aplicativo, criar, editar e excluir um contato</span><span class="sxs-lookup"><span data-stu-id="32910-269">Test the app by creating, editing and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="32910-270">Propagar o banco de dados</span><span class="sxs-lookup"><span data-stu-id="32910-270">Seed the database</span></span>

<span data-ttu-id="32910-271">Adicionar o `SeedData` de classe para o *dados* pasta.</span><span class="sxs-lookup"><span data-stu-id="32910-271">Add the `SeedData` class to the *Data* folder.</span></span> <span data-ttu-id="32910-272">Se você baixou o exemplo, você pode copiar o *SeedData.cs* o arquivo para o *dados* pasta do projeto starter.</span><span class="sxs-lookup"><span data-stu-id="32910-272">If you've downloaded the sample, you can copy the *SeedData.cs* file to the *Data* folder of the starter project.</span></span>

[!code-csharp[Main](secure-data/samples/starter/Data/SeedData.cs)]

<span data-ttu-id="32910-273">Adicione o código realçado até o final do `Configure` método o *Startup.cs* arquivo:</span><span class="sxs-lookup"><span data-stu-id="32910-273">Add the highlighted code to the end of the `Configure` method in the *Startup.cs* file:</span></span>

[!code-csharp[Main](secure-data/samples/starter/Startup.cs?name=Configure&highlight=28-)]

<span data-ttu-id="32910-274">Teste o aplicativo propagado o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="32910-274">Test that the app seeded the database.</span></span> <span data-ttu-id="32910-275">O método de propagação não será executado se há quaisquer linhas no banco de dados de contato.</span><span class="sxs-lookup"><span data-stu-id="32910-275">The seed method does not run if there are any rows in the contact DB.</span></span>

### <a name="create-a-class-used-in-the-tutorial"></a><span data-ttu-id="32910-276">Criar uma classe usada no tutorial</span><span class="sxs-lookup"><span data-stu-id="32910-276">Create a class used in the tutorial</span></span>

* <span data-ttu-id="32910-277">Crie uma pasta chamada *autorização*.</span><span class="sxs-lookup"><span data-stu-id="32910-277">Create a folder named *Authorization*.</span></span>
* <span data-ttu-id="32910-278">Copie o *Authorization\ContactOperations.cs* do arquivo de download do projeto concluído ou copie o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="32910-278">Copy the *Authorization\ContactOperations.cs* file from the completed project download, or copy the following code:</span></span>

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactOperations.cs)]

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a><span data-ttu-id="32910-279">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="32910-279">Additional resources</span></span>

* <span data-ttu-id="32910-280">[Laboratório de autorização de ASP.NET Core](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span><span class="sxs-lookup"><span data-stu-id="32910-280">[ASP.NET Core Authorization Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span></span> <span data-ttu-id="32910-281">Este laboratório apresenta mais detalhes sobre os recursos de segurança introduzidos neste tutorial.</span><span class="sxs-lookup"><span data-stu-id="32910-281">This lab goes into more detail on the security features introduced in this tutorial.</span></span>
* [<span data-ttu-id="32910-282">Autorização no ASP.NET Core: Simple, função, baseada em declarações e personalizada</span><span class="sxs-lookup"><span data-stu-id="32910-282">Authorization in ASP.NET Core : Simple, role, claims-based and custom</span></span>](index.md)
* [<span data-ttu-id="32910-283">Autorização baseada em política personalizada</span><span class="sxs-lookup"><span data-stu-id="32910-283">Custom policy-based authorization</span></span>](policies.md)
