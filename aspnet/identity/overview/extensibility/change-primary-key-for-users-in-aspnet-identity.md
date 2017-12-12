---
uid: identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
title: "Alterar a chave primária para usuários no ASP.NET Identity | Microsoft Docs"
author: tfitzmac
description: "No Visual Studio 2013, o aplicativo web padrão usa um valor de cadeia de caracteres para a chave para as contas de usuário. Identidade do ASP.NET permite que você altere o tipo da..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/30/2014
ms.topic: article
ms.assetid: 44925849-5762-4504-a8cd-8f0cd06f6dc3
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 79812efb4de2461fad3765d6005bbd20393e62b2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="change-primary-key-for-users-in-aspnet-identity"></a><span data-ttu-id="c8ddd-104">Alterar a chave primária para usuários no ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="c8ddd-104">Change Primary Key for Users in ASP.NET Identity</span></span>
====================
<span data-ttu-id="c8ddd-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="c8ddd-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="c8ddd-106">No Visual Studio 2013, o aplicativo web padrão usa um valor de cadeia de caracteres para a chave para as contas de usuário.</span><span class="sxs-lookup"><span data-stu-id="c8ddd-106">In Visual Studio 2013, the default web application uses a string value for the key for user accounts.</span></span> <span data-ttu-id="c8ddd-107">Identidade do ASP.NET permite que você altere o tipo de chave para atender aos requisitos de dados.</span><span class="sxs-lookup"><span data-stu-id="c8ddd-107">ASP.NET Identity enables you to change the type of the key to meet your data requirements.</span></span> <span data-ttu-id="c8ddd-108">Por exemplo, você pode alterar o tipo da chave de uma cadeia de caracteres para um número inteiro.</span><span class="sxs-lookup"><span data-stu-id="c8ddd-108">For example, you can change the type of the key from a string to an integer.</span></span>
> 
> <span data-ttu-id="c8ddd-109">Este tópico mostra como começar com o padrão aplicativo web e altere a chave de conta de usuário para um número inteiro.</span><span class="sxs-lookup"><span data-stu-id="c8ddd-109">This topic shows how to start with the default web application and change the user account key to an integer.</span></span> <span data-ttu-id="c8ddd-110">Você pode usar as mesmas modificações para implementar qualquer tipo de chave em seu projeto.</span><span class="sxs-lookup"><span data-stu-id="c8ddd-110">You can use the same modifications to implement any type of key in your project.</span></span> <span data-ttu-id="c8ddd-111">Ele mostra como fazer essas alterações no aplicativo da web padrão, mas você pode aplicar modificações semelhantes a um aplicativo personalizado.</span><span class="sxs-lookup"><span data-stu-id="c8ddd-111">It shows how to make these changes in the default web application, but you could apply similar modifications to a customized application.</span></span> <span data-ttu-id="c8ddd-112">Ele mostra as alterações necessárias ao trabalhar com MVC ou Web Forms.</span><span class="sxs-lookup"><span data-stu-id="c8ddd-112">It shows the changes needed when working with MVC or Web Forms.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="c8ddd-113">Versões de software usadas no tutorial</span><span class="sxs-lookup"><span data-stu-id="c8ddd-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="c8ddd-114">Visual Studio 2013 Update 2 (ou posterior)</span><span class="sxs-lookup"><span data-stu-id="c8ddd-114">Visual Studio 2013 with Update 2 (or later)</span></span>
> - <span data-ttu-id="c8ddd-115">Identidade do ASP.NET 2.1 ou posterior</span><span class="sxs-lookup"><span data-stu-id="c8ddd-115">ASP.NET Identity 2.1 or later</span></span>


<span data-ttu-id="c8ddd-116">Para executar as etapas neste tutorial, você deve ter o Visual Studio 2013 atualização 2 (ou posterior) e um aplicativo da web criados usando o modelo de aplicativo Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c8ddd-116">To perform the steps in this tutorial, you must have Visual Studio 2013 Update 2 (or later), and a web application created from the ASP.NET Web Application template.</span></span> <span data-ttu-id="c8ddd-117">O modelo foi alterado na atualização 3.</span><span class="sxs-lookup"><span data-stu-id="c8ddd-117">The template changed in Update 3.</span></span> <span data-ttu-id="c8ddd-118">Este tópico mostra como alterar o modelo na atualização 2 e 3.</span><span class="sxs-lookup"><span data-stu-id="c8ddd-118">This topic shows how to change the template in Update 2 and Update 3.</span></span>

<span data-ttu-id="c8ddd-119">Esse tópico contém as seguintes seções:</span><span class="sxs-lookup"><span data-stu-id="c8ddd-119">This topic contains the following sections:</span></span>

- [<span data-ttu-id="c8ddd-120">Alterar o tipo de chave da classe de usuário de identidade</span><span class="sxs-lookup"><span data-stu-id="c8ddd-120">Change the type of the key in the Identity user class</span></span>](#userclass)
- [<span data-ttu-id="c8ddd-121">Adicionar classes de identidade personalizadas que usam o tipo de chave</span><span class="sxs-lookup"><span data-stu-id="c8ddd-121">Add customized Identity classes that use the key type</span></span>](#customclass)
- [<span data-ttu-id="c8ddd-122">Alterar o Gerenciador de usuário e de classe de contexto para usar o tipo de chave</span><span class="sxs-lookup"><span data-stu-id="c8ddd-122">Change the context class and user manager to use the key type</span></span>](#context)
- [<span data-ttu-id="c8ddd-123">Alterar configuração de inicialização para usar o tipo de chave</span><span class="sxs-lookup"><span data-stu-id="c8ddd-123">Change start-up configuration to use the key type</span></span>](#startup)
- [<span data-ttu-id="c8ddd-124">Para MVC com atualização 2, altere o AccountController para passar o tipo de chave</span><span class="sxs-lookup"><span data-stu-id="c8ddd-124">For MVC with Update 2, change the AccountController to pass the key type</span></span>](#mvcupdate2)
- [<span data-ttu-id="c8ddd-125">Para MVC com atualização 3, altere o AccountController e ManageController para passar o tipo de chave</span><span class="sxs-lookup"><span data-stu-id="c8ddd-125">For MVC with Update 3, change the AccountController and ManageController to pass the key type</span></span>](#mvcupdate3)
- [<span data-ttu-id="c8ddd-126">Para formulários da Web com atualização 2, alterar as páginas de conta para passar o tipo de chave</span><span class="sxs-lookup"><span data-stu-id="c8ddd-126">For Web Forms with Update 2, change Account pages to pass the key type</span></span>](#webformsupdate2)
- [<span data-ttu-id="c8ddd-127">Para formulários da Web com atualização 3, alterar as páginas de conta para passar o tipo de chave</span><span class="sxs-lookup"><span data-stu-id="c8ddd-127">For Web Forms with Update 3, change Account pages to pass the key type</span></span>](#webformsupdate3)
- [<span data-ttu-id="c8ddd-128">Executar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="c8ddd-128">Run application</span></span>](#run)
- [<span data-ttu-id="c8ddd-129">Outros recursos</span><span class="sxs-lookup"><span data-stu-id="c8ddd-129">Other resources</span></span>](#other)

<a id="userclass"></a>
## <a name="change-the-type-of-the-key-in-the-identity-user-class"></a><span data-ttu-id="c8ddd-130">Alterar o tipo de chave da classe de usuário de identidade</span><span class="sxs-lookup"><span data-stu-id="c8ddd-130">Change the type of the key in the Identity user class</span></span>

<span data-ttu-id="c8ddd-131">No seu projeto criado usando o modelo de aplicativo Web ASP.NET, especifique que a classe ApplicationUser usa um número inteiro para a chave para as contas de usuário.</span><span class="sxs-lookup"><span data-stu-id="c8ddd-131">In your project created from the ASP.NET Web Application template, specify that the ApplicationUser class uses an integer for the key for user accounts.</span></span> <span data-ttu-id="c8ddd-132">Em IdentityModels.cs, altere a classe ApplicationUser para herdar de IdentityUser que tem um tipo de **int** para o parâmetro genérico TKey.</span><span class="sxs-lookup"><span data-stu-id="c8ddd-132">In IdentityModels.cs, change the ApplicationUser class to inherit from IdentityUser that has a type of **int** for the TKey generic parameter.</span></span> <span data-ttu-id="c8ddd-133">Você também pode passar os nomes dos três classe personalizada que ainda não tiver implementado.</span><span class="sxs-lookup"><span data-stu-id="c8ddd-133">You also pass the names of three customized class which you have not implemented yet.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample1.cs?highlight=1-2)]

<span data-ttu-id="c8ddd-134">Você alterou o tipo da chave, mas, por padrão, o restante do aplicativo ainda supõe que a chave é uma cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="c8ddd-134">You have changed the type of the key, but, by default, the rest of the application still assumes the key is a string.</span></span> <span data-ttu-id="c8ddd-135">Você deve indicar explicitamente o tipo de chave no código que assume uma cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="c8ddd-135">You must explicitly indicate the type of the key in code that assumes a string.</span></span>

<span data-ttu-id="c8ddd-136">No **ApplicationUser** classe, altere o **GenerateUserIdentityAsync** método incluir int, conforme mostrado no código realçado abaixo.</span><span class="sxs-lookup"><span data-stu-id="c8ddd-136">In the **ApplicationUser** class, change the **GenerateUserIdentityAsync** method to include int, as shown in the highlighted code below.</span></span> <span data-ttu-id="c8ddd-137">Essa alteração não é necessária para projetos Web Forms com o modelo de atualização 3.</span><span class="sxs-lookup"><span data-stu-id="c8ddd-137">This change is not necessary for Web Forms projects with the Update 3 template.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample2.cs?highlight=2)]

<a id="customclass"></a>
## <a name="add-customized-identity-classes-that-use-the-key-type"></a><span data-ttu-id="c8ddd-138">Adicionar classes de identidade personalizadas que usam o tipo de chave</span><span class="sxs-lookup"><span data-stu-id="c8ddd-138">Add customized Identity classes that use the key type</span></span>

<span data-ttu-id="c8ddd-139">As outras classes de identidade, como IdentityUserRole, IdentityUserClaim, IdentityUserLogin, IdentityRole, UserStore, RoleStore, ainda são configuradas para usar uma chave de cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="c8ddd-139">The other Identity classes, such as IdentityUserRole, IdentityUserClaim, IdentityUserLogin, IdentityRole, UserStore, RoleStore, are still set up to use a string key.</span></span> <span data-ttu-id="c8ddd-140">Crie novas versões dessas classes que especificam um número inteiro para a chave.</span><span class="sxs-lookup"><span data-stu-id="c8ddd-140">Create new versions of these classes that specify an integer for the key.</span></span> <span data-ttu-id="c8ddd-141">Você não precisa fornecer muito código de implementação nessas classes, principalmente apenas você está definindo int como a chave.</span><span class="sxs-lookup"><span data-stu-id="c8ddd-141">You do not need to provide much implementation code in these classes, you are primarily just setting int as the key.</span></span>

<span data-ttu-id="c8ddd-142">Adicione as seguintes classes no arquivo IdentityModels.cs.</span><span class="sxs-lookup"><span data-stu-id="c8ddd-142">Add the following classes to your IdentityModels.cs file.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample3.cs)]

<a id="context"></a>
## <a name="change-the-context-class-and-user-manager-to-use-the-key-type"></a><span data-ttu-id="c8ddd-143">Alterar o Gerenciador de usuário e de classe de contexto para usar o tipo de chave</span><span class="sxs-lookup"><span data-stu-id="c8ddd-143">Change the context class and user manager to use the key type</span></span>

<span data-ttu-id="c8ddd-144">Na IdentityModels.cs, altere a definição do **ApplicationDbContext** classe para usar a nova personalizado classes e um **int** para a chave, conforme mostrado no código realçado.</span><span class="sxs-lookup"><span data-stu-id="c8ddd-144">In IdentityModels.cs, change the definition of the **ApplicationDbContext** class to use your new customized classes and an **int** for the key, as shown in the highlighted code.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample4.cs?highlight=1-2)]

<span data-ttu-id="c8ddd-145">O parâmetro ThrowIfV1Schema não é válido no construtor.</span><span class="sxs-lookup"><span data-stu-id="c8ddd-145">The ThrowIfV1Schema parameter is no longer valid in the constructor.</span></span> <span data-ttu-id="c8ddd-146">Mudar o construtor, portanto, não passe um valor de ThrowIfV1Schema.</span><span class="sxs-lookup"><span data-stu-id="c8ddd-146">Change the constructor so it does not pass a ThrowIfV1Schema value.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample5.cs)]

<span data-ttu-id="c8ddd-147">Abra IdentityConfig.cs e altere o **ApplicationUserManger** classe para usar o novo usuário armazenar a classe para manter dados e um **int** para a chave.</span><span class="sxs-lookup"><span data-stu-id="c8ddd-147">Open IdentityConfig.cs, and change the **ApplicationUserManger** class to use your new user store class for persisting data and an **int** for the key.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample6.cs?highlight=1,3,12,14,32,37,48)]

<span data-ttu-id="c8ddd-148">No modelo de atualização 3, você deve alterar a classe ApplicationSignInManager.</span><span class="sxs-lookup"><span data-stu-id="c8ddd-148">In the Update 3 template, you must change the ApplicationSignInManager class.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample7.cs?highlight=1)]

<a id="startup"></a>
## <a name="change-start-up-configuration-to-use-the-key-type"></a><span data-ttu-id="c8ddd-149">Alterar configuração de inicialização para usar o tipo de chave</span><span class="sxs-lookup"><span data-stu-id="c8ddd-149">Change start-up configuration to use the key type</span></span>

<span data-ttu-id="c8ddd-150">No Startup.Auth.cs, substitua o código de OnValidateIdentity, como destacado abaixo.</span><span class="sxs-lookup"><span data-stu-id="c8ddd-150">In Startup.Auth.cs, replace the OnValidateIdentity code, as highlighted below.</span></span> <span data-ttu-id="c8ddd-151">Observe que a definição de getUserIdCallback, analisa o valor de cadeia de caracteres em um número inteiro.</span><span class="sxs-lookup"><span data-stu-id="c8ddd-151">Notice that the getUserIdCallback definition, parses the string value into an integer.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample8.cs?highlight=7-12)]

<span data-ttu-id="c8ddd-152">Se seu projeto não reconhece a implementação do **GetUserId** método, talvez seja necessário atualizar o pacote do NuGet do ASP.NET Identity à versão 2.1</span><span class="sxs-lookup"><span data-stu-id="c8ddd-152">If your project does not recognize the generic implementation of the **GetUserId** method, you may need to update the ASP.NET Identity NuGet package to version 2.1</span></span>

<span data-ttu-id="c8ddd-153">Você fez muitas alterações para as classes de infraestrutura usadas pela identidade do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c8ddd-153">You have made a lot of changes to the infrastructure classes used by ASP.NET Identity.</span></span> <span data-ttu-id="c8ddd-154">Se você tentar compilar o projeto, você observará vários erros.</span><span class="sxs-lookup"><span data-stu-id="c8ddd-154">If you try compiling the project, you will notice a lot of errors.</span></span> <span data-ttu-id="c8ddd-155">Felizmente, os erros restantes são semelhantes.</span><span class="sxs-lookup"><span data-stu-id="c8ddd-155">Fortunately, the remaining errors are all similar.</span></span> <span data-ttu-id="c8ddd-156">A classe de identidade espera um número inteiro para a chave, mas o controlador (ou um formulário da Web) está passando um valor de cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="c8ddd-156">The Identity class expects an integer for the key, but the controller (or Web Form) is passing a string value.</span></span> <span data-ttu-id="c8ddd-157">Em cada caso, você precisa converter de uma cadeia de caracteres para e inteiro chamando **GetUserId&lt;int&gt;**.</span><span class="sxs-lookup"><span data-stu-id="c8ddd-157">In each case, you need to convert from a string to and integer by calling **GetUserId&lt;int&gt;**.</span></span> <span data-ttu-id="c8ddd-158">Você pode trabalhar com a lista de erros de compilação ou siga as alterações a seguir.</span><span class="sxs-lookup"><span data-stu-id="c8ddd-158">You can either work through the error list from compilation or follow the changes below.</span></span>

<span data-ttu-id="c8ddd-159">As alterações restantes dependem do tipo de projeto que você está criando e qual a atualização que você instalou no Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c8ddd-159">The remaining changes depend on the type of project you are creating and which update you have installed in Visual Studio.</span></span> <span data-ttu-id="c8ddd-160">Você pode ir diretamente para a seção através dos links a seguir</span><span class="sxs-lookup"><span data-stu-id="c8ddd-160">You can go directly to the relevant section through the following links</span></span>

- [<span data-ttu-id="c8ddd-161">Para MVC com atualização 2, altere o AccountController para passar o tipo de chave</span><span class="sxs-lookup"><span data-stu-id="c8ddd-161">For MVC with Update 2, change the AccountController to pass the key type</span></span>](#mvcupdate2)
- [<span data-ttu-id="c8ddd-162">Para MVC com atualização 3, altere o AccountController e ManageController para passar o tipo de chave</span><span class="sxs-lookup"><span data-stu-id="c8ddd-162">For MVC with Update 3, change the AccountController and ManageController to pass the key type</span></span>](#mvcupdate3)
- [<span data-ttu-id="c8ddd-163">Para formulários da Web com atualização 2, alterar as páginas de conta para passar o tipo de chave</span><span class="sxs-lookup"><span data-stu-id="c8ddd-163">For Web Forms with Update 2, change Account pages to pass the key type</span></span>](#webformsupdate2)
- [<span data-ttu-id="c8ddd-164">Para formulários da Web com atualização 3, alterar as páginas de conta para passar o tipo de chave</span><span class="sxs-lookup"><span data-stu-id="c8ddd-164">For Web Forms with Update 3, change Account pages to pass the key type</span></span>](#webformsupdate3)

<a id="mvcupdate2"></a>
## <a name="for-mvc-with-update-2-change-the-accountcontroller-to-pass-the-key-type"></a><span data-ttu-id="c8ddd-165">Para MVC com atualização 2, altere o AccountController para passar o tipo de chave</span><span class="sxs-lookup"><span data-stu-id="c8ddd-165">For MVC with Update 2, change the AccountController to pass the key type</span></span>

<span data-ttu-id="c8ddd-166">Abra o arquivo AccountController.cs.</span><span class="sxs-lookup"><span data-stu-id="c8ddd-166">Open the AccountController.cs file.</span></span> <span data-ttu-id="c8ddd-167">Você precisa alterar os métodos a seguir.</span><span class="sxs-lookup"><span data-stu-id="c8ddd-167">You need to change the following methods.</span></span>

<span data-ttu-id="c8ddd-168">**ConfirmEmail** método</span><span class="sxs-lookup"><span data-stu-id="c8ddd-168">**ConfirmEmail** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample9.cs?highlight=1,3)]

<span data-ttu-id="c8ddd-169">**Desassociar** método</span><span class="sxs-lookup"><span data-stu-id="c8ddd-169">**Disassociate** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample10.cs?highlight=5,9)]

<span data-ttu-id="c8ddd-170">**Manage(ManageUserViewModel)** método</span><span class="sxs-lookup"><span data-stu-id="c8ddd-170">**Manage(ManageUserViewModel)** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample11.cs?highlight=11,17,41)]

<span data-ttu-id="c8ddd-171">**LinkLoginCallback** método</span><span class="sxs-lookup"><span data-stu-id="c8ddd-171">**LinkLoginCallback** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample12.cs?highlight=10)]

<span data-ttu-id="c8ddd-172">**RemoveAccountList** método</span><span class="sxs-lookup"><span data-stu-id="c8ddd-172">**RemoveAccountList** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample13.cs?highlight=3)]

<span data-ttu-id="c8ddd-173">**HasPassword** método</span><span class="sxs-lookup"><span data-stu-id="c8ddd-173">**HasPassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample14.cs?highlight=3)]

<span data-ttu-id="c8ddd-174">Agora você pode [executar o aplicativo](#run) e registrar um novo usuário.</span><span class="sxs-lookup"><span data-stu-id="c8ddd-174">You can now [run the application](#run) and register a new user.</span></span>

<a id="mvcupdate3"></a>
## <a name="for-mvc-with-update-3-change-the-accountcontroller-and-managecontroller-to-pass-the-key-type"></a><span data-ttu-id="c8ddd-175">Para MVC com atualização 3, altere o AccountController e ManageController para passar o tipo de chave</span><span class="sxs-lookup"><span data-stu-id="c8ddd-175">For MVC with Update 3, change the AccountController and ManageController to pass the key type</span></span>

<span data-ttu-id="c8ddd-176">Abra o arquivo AccountController.cs.</span><span class="sxs-lookup"><span data-stu-id="c8ddd-176">Open the AccountController.cs file.</span></span> <span data-ttu-id="c8ddd-177">Você precisa alterar o método a seguir.</span><span class="sxs-lookup"><span data-stu-id="c8ddd-177">You need to change the following method.</span></span>

<span data-ttu-id="c8ddd-178">**ConfirmEmail** método</span><span class="sxs-lookup"><span data-stu-id="c8ddd-178">**ConfirmEmail** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample15.cs?highlight=1,3)]

<span data-ttu-id="c8ddd-179">**SendCode** método</span><span class="sxs-lookup"><span data-stu-id="c8ddd-179">**SendCode** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample16.cs?highlight=4)]

<span data-ttu-id="c8ddd-180">Abra o arquivo ManageController.cs.</span><span class="sxs-lookup"><span data-stu-id="c8ddd-180">Open the ManageController.cs file.</span></span> <span data-ttu-id="c8ddd-181">Você precisa alterar os métodos a seguir.</span><span class="sxs-lookup"><span data-stu-id="c8ddd-181">You need to change the following methods.</span></span>

<span data-ttu-id="c8ddd-182">**Índice** método</span><span class="sxs-lookup"><span data-stu-id="c8ddd-182">**Index** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample17.cs?highlight=15-17)]

<span data-ttu-id="c8ddd-183">**RemoveLogin** métodos</span><span class="sxs-lookup"><span data-stu-id="c8ddd-183">**RemoveLogin** methods</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample18.cs?highlight=3,13,17)]

<span data-ttu-id="c8ddd-184">**AddPhoneNumber** método</span><span class="sxs-lookup"><span data-stu-id="c8ddd-184">**AddPhoneNumber** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample19.cs?highlight=9)]

<span data-ttu-id="c8ddd-185">**EnableTwoFactorAuthentication** método</span><span class="sxs-lookup"><span data-stu-id="c8ddd-185">**EnableTwoFactorAuthentication** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample20.cs?highlight=3-4)]

<span data-ttu-id="c8ddd-186">**DisableTwoFactorAuthentication** método</span><span class="sxs-lookup"><span data-stu-id="c8ddd-186">**DisableTwoFactorAuthentication** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample21.cs?highlight=3-4)]

<span data-ttu-id="c8ddd-187">**VerifyPhoneNumber** métodos</span><span class="sxs-lookup"><span data-stu-id="c8ddd-187">**VerifyPhoneNumber** methods</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample22.cs?highlight=4,18,21)]

<span data-ttu-id="c8ddd-188">**RemovePhoneNumber** método</span><span class="sxs-lookup"><span data-stu-id="c8ddd-188">**RemovePhoneNumber** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample23.cs?highlight=3,8)]

<span data-ttu-id="c8ddd-189">**ChangePassword** método</span><span class="sxs-lookup"><span data-stu-id="c8ddd-189">**ChangePassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample24.cs?highlight=10,13)]

<span data-ttu-id="c8ddd-190">**SetPassword** método</span><span class="sxs-lookup"><span data-stu-id="c8ddd-190">**SetPassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample25.cs?highlight=5,8)]

<span data-ttu-id="c8ddd-191">**ManageLogins** método</span><span class="sxs-lookup"><span data-stu-id="c8ddd-191">**ManageLogins** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample26.cs?highlight=7,12)]

<span data-ttu-id="c8ddd-192">**LinkLoginCallback** método</span><span class="sxs-lookup"><span data-stu-id="c8ddd-192">**LinkLoginCallback** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample27.cs?highlight=8)]

<span data-ttu-id="c8ddd-193">**HasPassword** método</span><span class="sxs-lookup"><span data-stu-id="c8ddd-193">**HasPassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample28.cs?highlight=3)]

<span data-ttu-id="c8ddd-194">**HasPhoneNumber** método</span><span class="sxs-lookup"><span data-stu-id="c8ddd-194">**HasPhoneNumber** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample29.cs?highlight=3)]

<span data-ttu-id="c8ddd-195">Agora você pode [executar o aplicativo](#run) e registrar um novo usuário.</span><span class="sxs-lookup"><span data-stu-id="c8ddd-195">You can now [run the application](#run) and register a new user.</span></span>

<a id="webformsupdate2"></a>
## <a name="for-web-forms-with-update-2-change-account-pages-to-pass-the-key-type"></a><span data-ttu-id="c8ddd-196">Para formulários da Web com atualização 2, alterar as páginas de conta para passar o tipo de chave</span><span class="sxs-lookup"><span data-stu-id="c8ddd-196">For Web Forms with Update 2, change Account pages to pass the key type</span></span>

<span data-ttu-id="c8ddd-197">Para formulários da Web com atualização 2, você precisa alterar as páginas a seguir.</span><span class="sxs-lookup"><span data-stu-id="c8ddd-197">For Web Forms with Update 2, you need to change the following pages.</span></span>

<span data-ttu-id="c8ddd-198">**Confirm.aspx.CX**</span><span class="sxs-lookup"><span data-stu-id="c8ddd-198">**Confirm.aspx.cx**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample30.cs?highlight=8)]

<span data-ttu-id="c8ddd-199">**RegisterExternalLogin.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="c8ddd-199">**RegisterExternalLogin.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample31.cs?highlight=36)]

<span data-ttu-id="c8ddd-200">**Manage.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="c8ddd-200">**Manage.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample32.cs?highlight=3,22,47,52,69,85,93,98)]

<span data-ttu-id="c8ddd-201">Agora você pode [executar o aplicativo](#run) e registrar um novo usuário.</span><span class="sxs-lookup"><span data-stu-id="c8ddd-201">You can now [run the application](#run) and register a new user.</span></span>

<a id="webformsupdate3"></a>
## <a name="for-web-forms-with-update-3-change-account-pages-to-pass-the-key-type"></a><span data-ttu-id="c8ddd-202">Para formulários da Web com atualização 3, alterar as páginas de conta para passar o tipo de chave</span><span class="sxs-lookup"><span data-stu-id="c8ddd-202">For Web Forms with Update 3, change Account pages to pass the key type</span></span>

<span data-ttu-id="c8ddd-203">Para formulários da Web com atualização 3, você precisa alterar as páginas a seguir.</span><span class="sxs-lookup"><span data-stu-id="c8ddd-203">For Web Forms with Update 3, you need to change the following pages.</span></span>

<span data-ttu-id="c8ddd-204">**Confirm.aspx.CX**</span><span class="sxs-lookup"><span data-stu-id="c8ddd-204">**Confirm.aspx.cx**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample33.cs?highlight=8)]

<span data-ttu-id="c8ddd-205">**RegisterExternalLogin.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="c8ddd-205">**RegisterExternalLogin.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample34.cs?highlight=36)]

<span data-ttu-id="c8ddd-206">**Manage.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="c8ddd-206">**Manage.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample35.cs?highlight=11,27,32,34,82,87,99,108)]

<span data-ttu-id="c8ddd-207">**VerifyPhoneNumber.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="c8ddd-207">**VerifyPhoneNumber.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample36.cs?highlight=8,23,27)]

<span data-ttu-id="c8ddd-208">**AddPhoneNumber.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="c8ddd-208">**AddPhoneNumber.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample37.cs?highlight=7)]

<span data-ttu-id="c8ddd-209">**ManagePassword.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="c8ddd-209">**ManagePassword.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample38.cs?highlight=11,47,50,68)]

<span data-ttu-id="c8ddd-210">**ManageLogins.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="c8ddd-210">**ManageLogins.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample39.cs?highlight=16,23,32,41,45)]

<span data-ttu-id="c8ddd-211">**TwoFactorAuthenticationSignIn.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="c8ddd-211">**TwoFactorAuthenticationSignIn.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample40.cs?highlight=14-15,29,53)]

<a id="run"></a>
## <a name="run-application"></a><span data-ttu-id="c8ddd-212">Executar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="c8ddd-212">Run application</span></span>

<span data-ttu-id="c8ddd-213">Você concluiu todas as alterações necessárias para o modelo de aplicativo da Web padrão.</span><span class="sxs-lookup"><span data-stu-id="c8ddd-213">You have finished all of the required changes to the default Web Application template.</span></span> <span data-ttu-id="c8ddd-214">Execute o aplicativo e registrar um novo usuário.</span><span class="sxs-lookup"><span data-stu-id="c8ddd-214">Run the application and register a new user.</span></span> <span data-ttu-id="c8ddd-215">Depois de registrar o usuário, você observará que a tabela de AspNetUsers possui uma coluna de Id que é um número inteiro.</span><span class="sxs-lookup"><span data-stu-id="c8ddd-215">After registering the user, you will notice that the AspNetUsers table has an Id column that is an integer.</span></span>

![nova chave primária](change-primary-key-for-users-in-aspnet-identity/_static/image1.png)

<span data-ttu-id="c8ddd-217">Se você tiver criado anteriormente a identidade do ASP.NET tabelas com uma chave primária diferente, você precisa fazer algumas alterações adicionais.</span><span class="sxs-lookup"><span data-stu-id="c8ddd-217">If you have previously created the ASP.NET Identity tables with a different primary key, you need to make some additional changes.</span></span> <span data-ttu-id="c8ddd-218">Se possível, exclua o banco de dados existente.</span><span class="sxs-lookup"><span data-stu-id="c8ddd-218">If possible, just delete the existing database.</span></span> <span data-ttu-id="c8ddd-219">O banco de dados será recriado com o design correto ao executar o aplicativo web e adicionar um novo usuário.</span><span class="sxs-lookup"><span data-stu-id="c8ddd-219">The database will be re-created with the correct design when you run the web application and add a new user.</span></span> <span data-ttu-id="c8ddd-220">Se a exclusão não é possível executar migrações do code first para alterar as tabelas.</span><span class="sxs-lookup"><span data-stu-id="c8ddd-220">If deletion is not possible, run code first migrations to change the tables.</span></span> <span data-ttu-id="c8ddd-221">No entanto, a nova chave primária de inteiro será não ser configurada como uma propriedade de identidade de SQL no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="c8ddd-221">However, the new integer primary key will not be set up as a SQL IDENTITY property in the database.</span></span> <span data-ttu-id="c8ddd-222">Você deve definir manualmente a coluna Id como uma identidade.</span><span class="sxs-lookup"><span data-stu-id="c8ddd-222">You must manually set the Id column as an IDENTITY.</span></span>

<a id="other"></a>
## <a name="other-resources"></a><span data-ttu-id="c8ddd-223">Outros recursos</span><span class="sxs-lookup"><span data-stu-id="c8ddd-223">Other resources</span></span>

- [<span data-ttu-id="c8ddd-224">Visão geral dos provedores de armazenamento personalizado para a identidade do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c8ddd-224">Overview of Custom Storage Providers for ASP.NET Identity</span></span>](overview-of-custom-storage-providers-for-aspnet-identity.md)
- [<span data-ttu-id="c8ddd-225">Migrando um site existente da associação SQL para a identidade do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c8ddd-225">Migrating an Existing Website from SQL Membership to ASP.NET Identity</span></span>](../migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md)
- [<span data-ttu-id="c8ddd-226">Migração de dados Universal do provedor de associação e os perfis de usuário para a identidade do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c8ddd-226">Migrating Universal Provider Data for Membership and User Profiles to ASP.NET Identity</span></span>](../migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity.md)
- <span data-ttu-id="c8ddd-227">[Aplicativo de exemplo](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/ChangePK/readme.txt) com a chave primária alterada</span><span class="sxs-lookup"><span data-stu-id="c8ddd-227">[Sample application](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/ChangePK/readme.txt) with changed primary key</span></span>
