---
title: "Autorização com base em recursos"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 0902ba17-5304-4a12-a2d4-e0904569e988
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/resourcebased
ms.openlocfilehash: 2f799588ba4aca4664e1679e4c34657e7ca121fb
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2017
---
# <a name="resource-based-authorization"></a><span data-ttu-id="18110-103">Autorização com base em recursos</span><span class="sxs-lookup"><span data-stu-id="18110-103">Resource Based Authorization</span></span>

<a name=security-authorization-resource-based></a>

<span data-ttu-id="18110-104">Autorização geralmente depende do recurso que está sendo acessado.</span><span class="sxs-lookup"><span data-stu-id="18110-104">Often authorization depends upon the resource being accessed.</span></span> <span data-ttu-id="18110-105">Por exemplo, um documento pode ter uma propriedade de autor.</span><span class="sxs-lookup"><span data-stu-id="18110-105">For example a document may have an author property.</span></span> <span data-ttu-id="18110-106">Somente o autor do documento deve ter permissão para atualizá-lo, para que o recurso deve ser carregado do repositório do documento antes de uma avaliação de autorização pode ser feita.</span><span class="sxs-lookup"><span data-stu-id="18110-106">Only the document author would be allowed to update it, so the resource must be loaded from the document repository before an authorization evaluation can be made.</span></span> <span data-ttu-id="18110-107">Isso não pode ser feito com um atributo de autorização, como avaliação de atributo ocorre antes da associação de dados e antes de executar seu próprio código para carregar um recurso dentro de uma ação.</span><span class="sxs-lookup"><span data-stu-id="18110-107">This cannot be done with an Authorize attribute, as attribute evaluation takes place before data binding and before your own code to load a resource runs inside an action.</span></span> <span data-ttu-id="18110-108">Em vez de autorização declarativa, o método de atributo, podemos deve usar autorização obrigatória, onde um desenvolvedor chama uma função de autorização em seu próprio código.</span><span class="sxs-lookup"><span data-stu-id="18110-108">Instead of declarative authorization, the attribute method, we must use imperative authorization, where a developer calls an authorize function within their own code.</span></span>

## <a name="authorizing-within-your-code"></a><span data-ttu-id="18110-109">Autorizando dentro de seu código</span><span class="sxs-lookup"><span data-stu-id="18110-109">Authorizing within your code</span></span>

<span data-ttu-id="18110-110">Autorização é implementada como um serviço, `IAuthorizationService`, registrados na coleção de serviço e disponível por meio de [injeção de dependência](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection) para controladores acessar.</span><span class="sxs-lookup"><span data-stu-id="18110-110">Authorization is implemented as a service, `IAuthorizationService`, registered in the service collection and available via [dependency injection](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection) for Controllers to access.</span></span>

```csharp
public class DocumentController : Controller
{
    IAuthorizationService _authorizationService;

    public DocumentController(IAuthorizationService authorizationService)
    {
        _authorizationService = authorizationService;
    }
}
```

<span data-ttu-id="18110-111">`IAuthorizationService`tem dois métodos, um em que você passa o recurso e o nome da política e outros em que você passa o recurso e uma lista de requisitos para avaliar.</span><span class="sxs-lookup"><span data-stu-id="18110-111">`IAuthorizationService` has two methods, one where you pass the resource and the policy name and the other where you pass the resource and a list of requirements to evaluate.</span></span>

```csharp
Task<bool> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          IEnumerable<IAuthorizationRequirement> requirements);
Task<bool> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          string policyName);
```

<a name=security-authorization-resource-based-imperative></a>

<span data-ttu-id="18110-112">Para chamar a carga do serviço seus recursos em sua ação, em seguida, chame o `AuthorizeAsync` sobrecarga que você precisa.</span><span class="sxs-lookup"><span data-stu-id="18110-112">To call the service load your resource within your action then call the `AuthorizeAsync` overload you require.</span></span> <span data-ttu-id="18110-113">Por exemplo</span><span class="sxs-lookup"><span data-stu-id="18110-113">For example</span></span>

```csharp
public async Task<IActionResult> Edit(Guid documentId)
{
    Document document = documentRepository.Find(documentId);

    if (document == null)
    {
        return new HttpNotFoundResult();
    }

    if (await _authorizationService.AuthorizeAsync(User, document, "EditPolicy"))
    {
        return View(document);
    }
    else
    {
        return new ChallengeResult();
    }
}
```

## <a name="writing-a-resource-based-handler"></a><span data-ttu-id="18110-114">Escrevendo um manipulador de recurso com base</span><span class="sxs-lookup"><span data-stu-id="18110-114">Writing a resource based handler</span></span>

<span data-ttu-id="18110-115">Escrevendo um manipulador de autorização de recursos com base não é muito diferente de [escrevendo um manipulador de requisitos simples](policies.md#security-authorization-policies-based-authorization-handler).</span><span class="sxs-lookup"><span data-stu-id="18110-115">Writing a handler for resource based authorization is not that much different to [writing a plain requirements handler](policies.md#security-authorization-policies-based-authorization-handler).</span></span> <span data-ttu-id="18110-116">Criar um requisito e, em seguida, implementar um manipulador para o requisito, especificando o requisito como antes e também o tipo de recurso.</span><span class="sxs-lookup"><span data-stu-id="18110-116">You create a requirement, and then implement a handler for the requirement, specifying the requirement as before and also the resource type.</span></span> <span data-ttu-id="18110-117">Por exemplo, um manipulador que pode aceitar um recurso de documento seria semelhante ao seguinte;</span><span class="sxs-lookup"><span data-stu-id="18110-117">For example, a handler which might accept a Document resource would look as follows;</span></span>

```csharp
public class DocumentAuthorizationHandler : AuthorizationHandler<MyRequirement, Document>
{
    public override Task HandleRequirementAsync(AuthorizationHandlerContext context,
                                                MyRequirement requirement,
                                                Document resource)
    {
        // Validate the requirement against the resource and identity.

        return Task.CompletedTask;
    }
}
```

<span data-ttu-id="18110-118">Não se esqueça de você também precisa registrar o manipulador no `ConfigureServices` método;</span><span class="sxs-lookup"><span data-stu-id="18110-118">Don't forget you also need to register your handler in the `ConfigureServices` method;</span></span>

```csharp
services.AddSingleton<IAuthorizationHandler, DocumentAuthorizationHandler>();
```

### <a name="operational-requirements"></a><span data-ttu-id="18110-119">Requisitos operacionais</span><span class="sxs-lookup"><span data-stu-id="18110-119">Operational Requirements</span></span>

<span data-ttu-id="18110-120">Se você estiver fazendo decisões com base em operações, como leitura, gravação, atualização e exclusão, você pode usar o `OperationAuthorizationRequirement` classe no `Microsoft.AspNetCore.Authorization.Infrastructure` namespace.</span><span class="sxs-lookup"><span data-stu-id="18110-120">If you are making decisions based on operations such as read, write, update and delete, you can use the `OperationAuthorizationRequirement` class in the `Microsoft.AspNetCore.Authorization.Infrastructure` namespace.</span></span> <span data-ttu-id="18110-121">Essa classe de requisito predefinido permite que você escrever um único manipulador que possui um nome de operação com parâmetros, em vez de criar classes individuais para cada operação.</span><span class="sxs-lookup"><span data-stu-id="18110-121">This prebuilt requirement class enables you to write a single handler which has a parameterized operation name, rather than create individual classes for each operation.</span></span> <span data-ttu-id="18110-122">Para usar, ele fornece alguns nomes de operação:</span><span class="sxs-lookup"><span data-stu-id="18110-122">To use it provide some operation names:</span></span>

```csharp
public static class Operations
{
    public static OperationAuthorizationRequirement Create =
        new OperationAuthorizationRequirement { Name = "Create" };
    public static OperationAuthorizationRequirement Read =
        new OperationAuthorizationRequirement   { Name = "Read" };
    public static OperationAuthorizationRequirement Update =
        new OperationAuthorizationRequirement { Name = "Update" };
    public static OperationAuthorizationRequirement Delete =
        new OperationAuthorizationRequirement { Name = "Delete" };
}
```

<span data-ttu-id="18110-123">O manipulador pode, em seguida, ser implementado como a seguir, usando um hipotético `Document` classe como o recurso.</span><span class="sxs-lookup"><span data-stu-id="18110-123">Your handler could then be implemented as follows, using a hypothetical `Document` class as the resource;</span></span>

```csharp
public class DocumentAuthorizationHandler :
    AuthorizationHandler<OperationAuthorizationRequirement, Document>
{
    public override Task HandleRequirementAsync(AuthorizationHandlerContext context,
                                                OperationAuthorizationRequirement requirement,
                                                Document resource)
    {
        // Validate the operation using the resource, the identity and
        // the Name property value from the requirement.

        return Task.CompletedTask;
    }
}
```

<span data-ttu-id="18110-124">Você pode ver a manipulador funciona em `OperationAuthorizationRequirement`.</span><span class="sxs-lookup"><span data-stu-id="18110-124">You can see the handler works on `OperationAuthorizationRequirement`.</span></span> <span data-ttu-id="18110-125">O código dentro do manipulador de deve levar a propriedade de nome do requisito fornecido em conta ao fazer seus avaliações.</span><span class="sxs-lookup"><span data-stu-id="18110-125">The code inside the handler must take the Name property of the supplied requirement into account when making its evaluations.</span></span>

<span data-ttu-id="18110-126">Para chamar um manipulador de recurso operacionais que você precisa especificar a operação ao chamar `AuthorizeAsync` em sua ação.</span><span class="sxs-lookup"><span data-stu-id="18110-126">To call an operational resource handler you need to specify the operation when calling `AuthorizeAsync` in your action.</span></span> <span data-ttu-id="18110-127">Por exemplo</span><span class="sxs-lookup"><span data-stu-id="18110-127">For example</span></span>

```csharp
if (await _authorizationService.AuthorizeAsync(User, document, Operations.Read))
{
    return View(document);
}
else
{
    return new ChallengeResult();
}
```

<span data-ttu-id="18110-128">Este exemplo verifica se o usuário é capaz de executar a operação de leitura para a atual `document` instância.</span><span class="sxs-lookup"><span data-stu-id="18110-128">This example checks if the User is able to perform the Read operation for the current `document` instance.</span></span> <span data-ttu-id="18110-129">Se a autorização bem-sucedida a exibição para o documento será retornada.</span><span class="sxs-lookup"><span data-stu-id="18110-129">If authorization succeeds the view for the document will be returned.</span></span> <span data-ttu-id="18110-130">Se a autorização falhar Retornando `ChallengeResult` informará qualquer autenticação, autorização de middleware falhou e o middleware pode levar a resposta apropriada, por exemplo retornando um código de status 401 ou 403 ou redirecionar o usuário para uma página de logon para clientes de navegador interativo.</span><span class="sxs-lookup"><span data-stu-id="18110-130">If authorization fails returning `ChallengeResult` will inform any authentication middleware authorization has failed and the middleware can take the appropriate response, for example returning a 401 or 403 status code, or redirecting the user to a login page for interactive browser clients.</span></span>
