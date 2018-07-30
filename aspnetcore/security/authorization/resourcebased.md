---
title: Autorização baseada em recursos no ASP.NET Core
author: scottaddie
description: Saiba como implementar a autorização baseada em recursos em um aplicativo ASP.NET Core quando um atributo Authorize não serão suficientes.
ms.author: scaddie
ms.custom: mvc
ms.date: 11/07/2017
uid: security/authorization/resourcebased
ms.openlocfilehash: 6a110a69c58d5e20a15198378510486daec3d452
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342283"
---
# <a name="resource-based-authorization-in-aspnet-core"></a><span data-ttu-id="e05e6-103">Autorização baseada em recursos no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e05e6-103">Resource-based authorization in ASP.NET Core</span></span>

<span data-ttu-id="e05e6-104">Estratégia de autorização depende do recurso que está sendo acessado.</span><span class="sxs-lookup"><span data-stu-id="e05e6-104">Authorization strategy depends upon the resource being accessed.</span></span> <span data-ttu-id="e05e6-105">Considere um documento que tem uma propriedade de autor.</span><span class="sxs-lookup"><span data-stu-id="e05e6-105">Consider a document which has an author property.</span></span> <span data-ttu-id="e05e6-106">Somente o autor tem permissão para atualizar o documento.</span><span class="sxs-lookup"><span data-stu-id="e05e6-106">Only the author is allowed to update the document.</span></span> <span data-ttu-id="e05e6-107">Consequentemente, o documento deve ser recuperado do armazenamento de dados antes que a avaliação de autorização pode ocorrer.</span><span class="sxs-lookup"><span data-stu-id="e05e6-107">Consequently, the document must be retrieved from the data store before authorization evaluation can occur.</span></span>

<span data-ttu-id="e05e6-108">Avaliação de atributo ocorre antes da vinculação de dados e antes da execução do manipulador de página ou ação que carrega o documento.</span><span class="sxs-lookup"><span data-stu-id="e05e6-108">Attribute evaluation occurs before data binding and before execution of the page handler or action which loads the document.</span></span> <span data-ttu-id="e05e6-109">Por esses motivos, a autorização declarativa com um `[Authorize]` atributo não serão suficientes.</span><span class="sxs-lookup"><span data-stu-id="e05e6-109">For these reasons, declarative authorization with an `[Authorize]` attribute won't suffice.</span></span> <span data-ttu-id="e05e6-110">Em vez disso, você pode invocar um método de autorização personalizada&mdash;um estilo conhecido como autorização imperativa.</span><span class="sxs-lookup"><span data-stu-id="e05e6-110">Instead, you can invoke a custom authorization method&mdash;a style known as imperative authorization.</span></span>

<span data-ttu-id="e05e6-111">Use o [aplicativos de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples) ([como baixar](xref:tutorials/index#how-to-download-a-sample)) para explorar os recursos descritos neste tópico.</span><span class="sxs-lookup"><span data-stu-id="e05e6-111">Use the [sample apps](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample)) to explore the features described in this topic.</span></span>

<span data-ttu-id="e05e6-112">[Criar um aplicativo ASP.NET Core com os dados de usuário protegidos por autorização](xref:security/authorization/secure-data) contém um aplicativo de exemplo que usa a autorização baseada em recursos.</span><span class="sxs-lookup"><span data-stu-id="e05e6-112">[Create an ASP.NET Core app with user data protected by authorization](xref:security/authorization/secure-data) contains a sample app that uses resource-based authorization.</span></span>

## <a name="use-imperative-authorization"></a><span data-ttu-id="e05e6-113">Usar a autorização obrigatória</span><span class="sxs-lookup"><span data-stu-id="e05e6-113">Use imperative authorization</span></span>

<span data-ttu-id="e05e6-114">Autorização é implementada como um [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) de serviço e é registrado na coleção de serviço dentro de `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="e05e6-114">Authorization is implemented as an [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) service and is registered in the service collection within the `Startup` class.</span></span> <span data-ttu-id="e05e6-115">O serviço é disponibilizado por meio [injeção de dependência](xref:fundamentals/dependency-injection) para manipuladores de página ou de ações.</span><span class="sxs-lookup"><span data-stu-id="e05e6-115">The service is made available via [dependency injection](xref:fundamentals/dependency-injection) to page handlers or actions.</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Controllers/DocumentController.cs?name=snippet_IAuthServiceDI&highlight=6)]

<span data-ttu-id="e05e6-116">`IAuthorizationService` tem dois `AuthorizeAsync` sobrecargas de método: uma aceitando o recurso e o nome da política e a outra aceitando o recurso e uma lista de requisitos para avaliar.</span><span class="sxs-lookup"><span data-stu-id="e05e6-116">`IAuthorizationService` has two `AuthorizeAsync` method overloads: one accepting the resource and the policy name and the other accepting the resource and a list of requirements to evaluate.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e05e6-117">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e05e6-117">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          IEnumerable<IAuthorizationRequirement> requirements);
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          string policyName);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e05e6-118">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e05e6-118">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
Task<bool> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          IEnumerable<IAuthorizationRequirement> requirements);
Task<bool> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          string policyName);
```

---

<a name="security-authorization-resource-based-imperative"></a>

<span data-ttu-id="e05e6-119">O exemplo a seguir, o recurso a ser protegido é carregado em um personalizado `Document` objeto.</span><span class="sxs-lookup"><span data-stu-id="e05e6-119">In the following example, the resource to be secured is loaded into a custom `Document` object.</span></span> <span data-ttu-id="e05e6-120">Um `AuthorizeAsync` sobrecarga é invocada para determinar se o usuário atual tem permissão para editar o documento.</span><span class="sxs-lookup"><span data-stu-id="e05e6-120">An `AuthorizeAsync` overload is invoked to determine whether the current user is allowed to edit the provided document.</span></span> <span data-ttu-id="e05e6-121">Uma política de autorização personalizada "EditPolicy" é acrescentada a uma decisão.</span><span class="sxs-lookup"><span data-stu-id="e05e6-121">A custom "EditPolicy" authorization policy is factored into the decision.</span></span> <span data-ttu-id="e05e6-122">Ver [autorização de baseado em políticas personalizadas](xref:security/authorization/policies) para obter mais informações sobre como criar políticas de autorização.</span><span class="sxs-lookup"><span data-stu-id="e05e6-122">See [Custom policy-based authorization](xref:security/authorization/policies) for more on creating authorization policies.</span></span>

> [!NOTE]
> <span data-ttu-id="e05e6-123">O código a seguir exemplos pressupõem que a autenticação foi executada e o conjunto de `User` propriedade.</span><span class="sxs-lookup"><span data-stu-id="e05e6-123">The following code samples assume authentication has run and set the `User` property.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e05e6-124">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e05e6-124">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/Edit.cshtml.cs?name=snippet_DocumentEditHandler)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e05e6-125">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e05e6-125">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentEditAction)]

---

## <a name="write-a-resource-based-handler"></a><span data-ttu-id="e05e6-126">Escrever um Gerenciador de recursos</span><span class="sxs-lookup"><span data-stu-id="e05e6-126">Write a resource-based handler</span></span>

<span data-ttu-id="e05e6-127">Escrevendo um manipulador para a autorização baseada em recursos não é muito diferente [escrevendo um manipulador de requisitos simples](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler).</span><span class="sxs-lookup"><span data-stu-id="e05e6-127">Writing a handler for resource-based authorization isn't much different than [writing a plain requirements handler](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler).</span></span> <span data-ttu-id="e05e6-128">Criar uma classe de requisito personalizado e implementar uma classe de manipulador de requisito.</span><span class="sxs-lookup"><span data-stu-id="e05e6-128">Create a custom requirement class, and implement a requirement handler class.</span></span> <span data-ttu-id="e05e6-129">A classe de manipulador Especifica o requisito e o tipo de recurso.</span><span class="sxs-lookup"><span data-stu-id="e05e6-129">The handler class specifies both the requirement and resource type.</span></span> <span data-ttu-id="e05e6-130">Por exemplo, uma utilização de manipulador uma `SameAuthorRequirement` requisito e um `Document` recursos será semelhante ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="e05e6-130">For example, a handler utilizing a `SameAuthorRequirement` requirement and a `Document` resource looks as follows:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e05e6-131">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e05e6-131">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e05e6-132">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e05e6-132">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

---

<span data-ttu-id="e05e6-133">Registrar o requisito e o manipulador no `Startup.ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="e05e6-133">Register the requirement and handler in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Startup.cs?name=snippet_ConfigureServicesSample&highlight=3-7,9)]

### <a name="operational-requirements"></a><span data-ttu-id="e05e6-134">Requisitos operacionais</span><span class="sxs-lookup"><span data-stu-id="e05e6-134">Operational requirements</span></span>

<span data-ttu-id="e05e6-135">Se você estiver fazendo decisões com base nos resultados de operações CRUD (Create, Read, Update, Delete), use o [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) classe auxiliar.</span><span class="sxs-lookup"><span data-stu-id="e05e6-135">If you're making decisions based on the outcomes of CRUD (Create, Read, Update, Delete) operations, use the [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) helper class.</span></span> <span data-ttu-id="e05e6-136">Essa classe permite que você escreva um único manipulador em vez de uma classe individual para cada tipo de operação.</span><span class="sxs-lookup"><span data-stu-id="e05e6-136">This class enables you to write a single handler instead of an individual class for each operation type.</span></span> <span data-ttu-id="e05e6-137">Para usá-lo, forneça alguns nomes de operação:</span><span class="sxs-lookup"><span data-stu-id="e05e6-137">To use it, provide some operation names:</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_OperationsClass)]

<span data-ttu-id="e05e6-138">O manipulador é implementado da seguinte maneira, usando um `OperationAuthorizationRequirement` requisito e um `Document` recursos:</span><span class="sxs-lookup"><span data-stu-id="e05e6-138">The handler is implemented as follows, using an `OperationAuthorizationRequirement` requirement and a `Document` resource:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e05e6-139">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e05e6-139">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e05e6-140">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e05e6-140">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

---

<span data-ttu-id="e05e6-141">O manipulador anterior valida a operação usando o recurso, a identidade do usuário e o requisito `Name` propriedade.</span><span class="sxs-lookup"><span data-stu-id="e05e6-141">The preceding handler validates the operation using the resource, the user's identity, and the requirement's `Name` property.</span></span>

<span data-ttu-id="e05e6-142">Para chamar um manipulador de recursos operacionais, especifique a operação ao invocar `AuthorizeAsync` em seu manipulador de página ou ação.</span><span class="sxs-lookup"><span data-stu-id="e05e6-142">To call an operational resource handler, specify the operation when invoking `AuthorizeAsync` in your page handler or action.</span></span> <span data-ttu-id="e05e6-143">O exemplo a seguir determina se o usuário autenticado tem permissão para exibir o documento.</span><span class="sxs-lookup"><span data-stu-id="e05e6-143">The following example determines whether the authenticated user is permitted to view the provided document.</span></span>

> [!NOTE]
> <span data-ttu-id="e05e6-144">O código a seguir exemplos pressupõem que a autenticação foi executada e o conjunto de `User` propriedade.</span><span class="sxs-lookup"><span data-stu-id="e05e6-144">The following code samples assume authentication has run and set the `User` property.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e05e6-145">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e05e6-145">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/View.cshtml.cs?name=snippet_DocumentViewHandler&highlight=10-11)]

<span data-ttu-id="e05e6-146">Se a autorização for bem-sucedida, a página para exibir o documento é retornada.</span><span class="sxs-lookup"><span data-stu-id="e05e6-146">If authorization succeeds, the page for viewing the document is returned.</span></span> <span data-ttu-id="e05e6-147">Se falhar de autorização, mas o usuário é autenticado, o retorno de `ForbidResult` informa qualquer middleware de autenticação que houve falha na autorização.</span><span class="sxs-lookup"><span data-stu-id="e05e6-147">If authorization fails but the user is authenticated, returning `ForbidResult` informs any authentication middleware that authorization failed.</span></span> <span data-ttu-id="e05e6-148">Um `ChallengeResult` é retornado quando a autenticação deve ser executada.</span><span class="sxs-lookup"><span data-stu-id="e05e6-148">A `ChallengeResult` is returned when authentication must be performed.</span></span> <span data-ttu-id="e05e6-149">Para clientes de navegador interativo, ele pode ser apropriado redirecionar o usuário para uma página de logon.</span><span class="sxs-lookup"><span data-stu-id="e05e6-149">For interactive browser clients, it may be appropriate to redirect the user to a login page.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e05e6-150">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e05e6-150">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentViewAction&highlight=11-12)]

<span data-ttu-id="e05e6-151">Se a autorização for bem-sucedida, o modo de exibição para o documento é retornado.</span><span class="sxs-lookup"><span data-stu-id="e05e6-151">If authorization succeeds, the view for the document is returned.</span></span> <span data-ttu-id="e05e6-152">Se a autorização falhar, retornando `ChallengeResult` informa qualquer middleware de autenticação que houve falha na autorização e o middleware pode levar a resposta apropriada.</span><span class="sxs-lookup"><span data-stu-id="e05e6-152">If authorization fails, returning `ChallengeResult` informs any authentication middleware that authorization failed, and the middleware can take the appropriate response.</span></span> <span data-ttu-id="e05e6-153">Uma resposta apropriada pode estar retornando um código de status 401 ou 403.</span><span class="sxs-lookup"><span data-stu-id="e05e6-153">An appropriate response could be returning a 401 or 403 status code.</span></span> <span data-ttu-id="e05e6-154">Para clientes de navegador interativo, isso poderia significar a redirecionar o usuário para uma página de logon.</span><span class="sxs-lookup"><span data-stu-id="e05e6-154">For interactive browser clients, it could mean redirecting the user to a login page.</span></span>

---
