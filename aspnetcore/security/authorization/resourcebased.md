---
title: Autorização baseada em recursos no ASP.NET Core
author: scottaddie
description: Aprenda a implementar a autorização baseada em recursos em um aplicativo do ASP.NET Core quando um atributo de autorização não é suficiente.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 11/07/2017
ms.devlang: csharp
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/resourcebased
ms.openlocfilehash: 5eac8ecf9de074d0a009690969de5beb4f284341
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
# <a name="resource-based-authorization-in-aspnet-core"></a><span data-ttu-id="009cc-103">Autorização baseada em recursos no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="009cc-103">Resource-based authorization in ASP.NET Core</span></span>

<span data-ttu-id="009cc-104">Estratégia de autorização depende do recurso que está sendo acessado.</span><span class="sxs-lookup"><span data-stu-id="009cc-104">Authorization strategy depends upon the resource being accessed.</span></span> <span data-ttu-id="009cc-105">Considere a possibilidade de um documento que tem uma propriedade de autor.</span><span class="sxs-lookup"><span data-stu-id="009cc-105">Consider a document which has an author property.</span></span> <span data-ttu-id="009cc-106">Somente o autor tem permissão para atualizar o documento.</span><span class="sxs-lookup"><span data-stu-id="009cc-106">Only the author is allowed to update the document.</span></span> <span data-ttu-id="009cc-107">Consequentemente, o documento deve ser recuperado do armazenamento de dados antes de avaliação de autorização pode ocorrer.</span><span class="sxs-lookup"><span data-stu-id="009cc-107">Consequently, the document must be retrieved from the data store before authorization evaluation can occur.</span></span>

<span data-ttu-id="009cc-108">Avaliação do atributo ocorre antes da associação de dados e antes da execução do manipulador de página ou ação que carrega o documento.</span><span class="sxs-lookup"><span data-stu-id="009cc-108">Attribute evaluation occurs before data binding and before execution of the page handler or action which loads the document.</span></span> <span data-ttu-id="009cc-109">Por esses motivos, autorização declarativa com um `[Authorize]` atributo não é suficiente.</span><span class="sxs-lookup"><span data-stu-id="009cc-109">For these reasons, declarative authorization with an `[Authorize]` attribute won't suffice.</span></span> <span data-ttu-id="009cc-110">Em vez disso, você pode chamar um método de autorização personalizada&mdash;um estilo conhecido como autorização obrigatória.</span><span class="sxs-lookup"><span data-stu-id="009cc-110">Instead, you can invoke a custom authorization method&mdash;a style known as imperative authorization.</span></span>

<span data-ttu-id="009cc-111">Use o [aplicativos de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples) ([como baixar](xref:tutorials/index#how-to-download-a-sample)) para explorar os recursos descritos neste tópico.</span><span class="sxs-lookup"><span data-stu-id="009cc-111">Use the [sample apps](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample)) to explore the features described in this topic.</span></span>

<span data-ttu-id="009cc-112">[Criar um aplicativo do ASP.NET Core com dados de usuário protegidos por autorização](xref:security/authorization/secure-data) contém um aplicativo de exemplo que usa a autorização baseada em recursos.</span><span class="sxs-lookup"><span data-stu-id="009cc-112">[Create an ASP.NET Core app with user data protected by authorization](xref:security/authorization/secure-data) contains a sample app that uses resource-based authorization.</span></span>

## <a name="use-imperative-authorization"></a><span data-ttu-id="009cc-113">Usar autorização obrigatória</span><span class="sxs-lookup"><span data-stu-id="009cc-113">Use imperative authorization</span></span>

<span data-ttu-id="009cc-114">Autorização é implementada como um [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) de serviço e é registrada na coleção de serviço dentro de `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="009cc-114">Authorization is implemented as an [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) service and is registered in the service collection within the `Startup` class.</span></span> <span data-ttu-id="009cc-115">O serviço é disponibilizado por meio de [injeção de dependência](xref:fundamentals/dependency-injection#fundamentals-dependency-injection) manipuladores de página ou ações.</span><span class="sxs-lookup"><span data-stu-id="009cc-115">The service is made available via [dependency injection](xref:fundamentals/dependency-injection#fundamentals-dependency-injection) to page handlers or actions.</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Controllers/DocumentController.cs?name=snippet_IAuthServiceDI&highlight=6)]

<span data-ttu-id="009cc-116">`IAuthorizationService` tem duas `AuthorizeAsync` sobrecargas do método: aceitando um recurso e o nome da política e o outro aceitando o recurso e uma lista de requisitos para avaliar.</span><span class="sxs-lookup"><span data-stu-id="009cc-116">`IAuthorizationService` has two `AuthorizeAsync` method overloads: one accepting the resource and the policy name and the other accepting the resource and a list of requirements to evaluate.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="009cc-117">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="009cc-117">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          IEnumerable<IAuthorizationRequirement> requirements);
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          string policyName);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="009cc-118">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="009cc-118">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

<span data-ttu-id="009cc-119">O exemplo a seguir, o recurso a ser protegida é carregado em um personalizado `Document` objeto.</span><span class="sxs-lookup"><span data-stu-id="009cc-119">In the following example, the resource to be secured is loaded into a custom `Document` object.</span></span> <span data-ttu-id="009cc-120">Um `AuthorizeAsync` sobrecarga é chamada para determinar se o usuário atual tem permissão para editar o documento.</span><span class="sxs-lookup"><span data-stu-id="009cc-120">An `AuthorizeAsync` overload is invoked to determine whether the current user is allowed to edit the provided document.</span></span> <span data-ttu-id="009cc-121">Uma diretiva de autorização personalizada "EditPolicy" é acrescentada a uma decisão.</span><span class="sxs-lookup"><span data-stu-id="009cc-121">A custom "EditPolicy" authorization policy is factored into the decision.</span></span> <span data-ttu-id="009cc-122">Consulte [autorização com base na política de personalizado](xref:security/authorization/policies) para obter mais informações sobre a criação de políticas de autorização.</span><span class="sxs-lookup"><span data-stu-id="009cc-122">See [Custom policy-based authorization](xref:security/authorization/policies) for more on creating authorization policies.</span></span>

> [!NOTE]
> <span data-ttu-id="009cc-123">O código a seguir exemplos pressupõem a autenticação foi executado e o conjunto de `User` propriedade.</span><span class="sxs-lookup"><span data-stu-id="009cc-123">The following code samples assume authentication has run and set the `User` property.</span></span>

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="009cc-124">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="009cc-124">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/Edit.cshtml.cs?name=snippet_DocumentEditHandler)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="009cc-125">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="009cc-125">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentEditAction)]

* * *
## <a name="write-a-resource-based-handler"></a><span data-ttu-id="009cc-126">Escrever um Gerenciador de recursos</span><span class="sxs-lookup"><span data-stu-id="009cc-126">Write a resource-based handler</span></span>

<span data-ttu-id="009cc-127">Escrevendo um manipulador para a autorização baseada em recursos não é muito diferente de [escrevendo um manipulador de requisitos simples](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler).</span><span class="sxs-lookup"><span data-stu-id="009cc-127">Writing a handler for resource-based authorization isn't much different than [writing a plain requirements handler](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler).</span></span> <span data-ttu-id="009cc-128">Criar uma classe personalizada de requisito e implementar uma classe de manipulador de requisito.</span><span class="sxs-lookup"><span data-stu-id="009cc-128">Create a custom requirement class, and implement a requirement handler class.</span></span> <span data-ttu-id="009cc-129">A classe do manipulador Especifica o requisito e o tipo de recurso.</span><span class="sxs-lookup"><span data-stu-id="009cc-129">The handler class specifies both the requirement and resource type.</span></span> <span data-ttu-id="009cc-130">Por exemplo, um uso de manipulador um `SameAuthorRequirement` requisito e um `Document` recurso tem a seguinte aparência:</span><span class="sxs-lookup"><span data-stu-id="009cc-130">For example, a handler utilizing a `SameAuthorRequirement` requirement and a `Document` resource looks as follows:</span></span>

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="009cc-131">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="009cc-131">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="009cc-132">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="009cc-132">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

* * *
<span data-ttu-id="009cc-133">Registrar o manipulador no e o requisito de `Startup.ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="009cc-133">Register the requirement and handler in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Startup.cs?name=snippet_ConfigureServicesSample&highlight=3-7,9)]

### <a name="operational-requirements"></a><span data-ttu-id="009cc-134">Requisitos operacionais</span><span class="sxs-lookup"><span data-stu-id="009cc-134">Operational requirements</span></span>

<span data-ttu-id="009cc-135">Se você estiver fazendo decisões com base nos resultados de CRUD (**C**riar, **R**ER, **U**tualizar, **D**excluir) operações, use o [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) classe auxiliar.</span><span class="sxs-lookup"><span data-stu-id="009cc-135">If you're making decisions based on the outcomes of CRUD (**C**reate, **R**ead, **U**pdate, **D**elete) operations, use the [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) helper class.</span></span> <span data-ttu-id="009cc-136">Essa classe permite que você grave um único manipulador em vez de uma classe individual para cada tipo de operação.</span><span class="sxs-lookup"><span data-stu-id="009cc-136">This class enables you to write a single handler instead of an individual class for each operation type.</span></span> <span data-ttu-id="009cc-137">Para usá-lo, forneça alguns nomes de operação:</span><span class="sxs-lookup"><span data-stu-id="009cc-137">To use it, provide some operation names:</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_OperationsClass)]

<span data-ttu-id="009cc-138">O manipulador é implementado como a seguir, usando um `OperationAuthorizationRequirement` requisito e um `Document` recursos:</span><span class="sxs-lookup"><span data-stu-id="009cc-138">The handler is implemented as follows, using an `OperationAuthorizationRequirement` requirement and a `Document` resource:</span></span>

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="009cc-139">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="009cc-139">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="009cc-140">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="009cc-140">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

* * *
<span data-ttu-id="009cc-141">O manipulador anterior valida a operação usando o recurso, a identidade do usuário e o requisito `Name` propriedade.</span><span class="sxs-lookup"><span data-stu-id="009cc-141">The preceding handler validates the operation using the resource, the user's identity, and the requirement's `Name` property.</span></span>

<span data-ttu-id="009cc-142">Para chamar um manipulador de recurso operacionais, especifique a operação ao invocar `AuthorizeAsync` no manipulador de página ou ação.</span><span class="sxs-lookup"><span data-stu-id="009cc-142">To call an operational resource handler, specify the operation when invoking `AuthorizeAsync` in your page handler or action.</span></span> <span data-ttu-id="009cc-143">O exemplo a seguir determina se o usuário autenticado tem permissão para exibir o documento.</span><span class="sxs-lookup"><span data-stu-id="009cc-143">The following example determines whether the authenticated user is permitted to view the provided document.</span></span>

> [!NOTE]
> <span data-ttu-id="009cc-144">O código a seguir exemplos pressupõem a autenticação foi executado e o conjunto de `User` propriedade.</span><span class="sxs-lookup"><span data-stu-id="009cc-144">The following code samples assume authentication has run and set the `User` property.</span></span>

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="009cc-145">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="009cc-145">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/View.cshtml.cs?name=snippet_DocumentViewHandler&highlight=10-11)]

<span data-ttu-id="009cc-146">Se a autorização bem-sucedida, a página para exibir o documento será retornada.</span><span class="sxs-lookup"><span data-stu-id="009cc-146">If authorization succeeds, the page for viewing the document is returned.</span></span> <span data-ttu-id="009cc-147">Se falhar de autorização, mas o usuário é autenticado, o retorno `ForbidResult` informa qualquer middleware de autenticação com falha de autorização.</span><span class="sxs-lookup"><span data-stu-id="009cc-147">If authorization fails but the user is authenticated, returning `ForbidResult` informs any authentication middleware that authorization failed.</span></span> <span data-ttu-id="009cc-148">Um `ChallengeResult` é retornado quando a autenticação deve ser executada.</span><span class="sxs-lookup"><span data-stu-id="009cc-148">A `ChallengeResult` is returned when authentication must be performed.</span></span> <span data-ttu-id="009cc-149">Para clientes de navegador interativo, pode ser apropriado redirecionar o usuário para uma página de logon.</span><span class="sxs-lookup"><span data-stu-id="009cc-149">For interactive browser clients, it may be appropriate to redirect the user to a login page.</span></span>

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="009cc-150">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="009cc-150">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentViewAction&highlight=11-12)]

<span data-ttu-id="009cc-151">Se a autorização bem-sucedida, o modo de exibição para o documento será retornado.</span><span class="sxs-lookup"><span data-stu-id="009cc-151">If authorization succeeds, the view for the document is returned.</span></span> <span data-ttu-id="009cc-152">Se a autorização falhar, retornando `ChallengeResult` informa qualquer middleware de autenticação que a falha de autorização e o middleware pode levar a resposta apropriada.</span><span class="sxs-lookup"><span data-stu-id="009cc-152">If authorization fails, returning `ChallengeResult` informs any authentication middleware that authorization failed, and the middleware can take the appropriate response.</span></span> <span data-ttu-id="009cc-153">Uma resposta apropriada pode estar retornando um código de status 401 ou 403.</span><span class="sxs-lookup"><span data-stu-id="009cc-153">An appropriate response could be returning a 401 or 403 status code.</span></span> <span data-ttu-id="009cc-154">Para clientes de navegador interativo, isso poderia significar redirecionar o usuário para uma página de logon.</span><span class="sxs-lookup"><span data-stu-id="009cc-154">For interactive browser clients, it could mean redirecting the user to a login page.</span></span>

* * *
