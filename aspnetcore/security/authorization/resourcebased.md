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
# <a name="resource-based-authorization"></a>Autorização com base em recursos

<a name=security-authorization-resource-based></a>

Autorização geralmente depende do recurso que está sendo acessado. Por exemplo, um documento pode ter uma propriedade de autor. Somente o autor do documento deve ter permissão para atualizá-lo, para que o recurso deve ser carregado do repositório do documento antes de uma avaliação de autorização pode ser feita. Isso não pode ser feito com um atributo de autorização, como avaliação de atributo ocorre antes da associação de dados e antes de executar seu próprio código para carregar um recurso dentro de uma ação. Em vez de autorização declarativa, o método de atributo, podemos deve usar autorização obrigatória, onde um desenvolvedor chama uma função de autorização em seu próprio código.

## <a name="authorizing-within-your-code"></a>Autorizando dentro de seu código

Autorização é implementada como um serviço, `IAuthorizationService`, registrados na coleção de serviço e disponível por meio de [injeção de dependência](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection) para controladores acessar.

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

`IAuthorizationService`tem dois métodos, um em que você passa o recurso e o nome da política e outros em que você passa o recurso e uma lista de requisitos para avaliar.

```csharp
Task<bool> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          IEnumerable<IAuthorizationRequirement> requirements);
Task<bool> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          string policyName);
```

<a name=security-authorization-resource-based-imperative></a>

Para chamar a carga do serviço seus recursos em sua ação, em seguida, chame o `AuthorizeAsync` sobrecarga que você precisa. Por exemplo

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

## <a name="writing-a-resource-based-handler"></a>Escrevendo um manipulador de recurso com base

Escrevendo um manipulador de autorização de recursos com base não é muito diferente de [escrevendo um manipulador de requisitos simples](policies.md#security-authorization-policies-based-authorization-handler). Criar um requisito e, em seguida, implementar um manipulador para o requisito, especificando o requisito como antes e também o tipo de recurso. Por exemplo, um manipulador que pode aceitar um recurso de documento seria semelhante ao seguinte;

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

Não se esqueça de você também precisa registrar o manipulador no `ConfigureServices` método;

```csharp
services.AddSingleton<IAuthorizationHandler, DocumentAuthorizationHandler>();
```

### <a name="operational-requirements"></a>Requisitos operacionais

Se você estiver fazendo decisões com base em operações, como leitura, gravação, atualização e exclusão, você pode usar o `OperationAuthorizationRequirement` classe no `Microsoft.AspNetCore.Authorization.Infrastructure` namespace. Essa classe de requisito predefinido permite que você escrever um único manipulador que possui um nome de operação com parâmetros, em vez de criar classes individuais para cada operação. Para usar, ele fornece alguns nomes de operação:

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

O manipulador pode, em seguida, ser implementado como a seguir, usando um hipotético `Document` classe como o recurso.

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

Você pode ver a manipulador funciona em `OperationAuthorizationRequirement`. O código dentro do manipulador de deve levar a propriedade de nome do requisito fornecido em conta ao fazer seus avaliações.

Para chamar um manipulador de recurso operacionais que você precisa especificar a operação ao chamar `AuthorizeAsync` em sua ação. Por exemplo

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

Este exemplo verifica se o usuário é capaz de executar a operação de leitura para a atual `document` instância. Se a autorização bem-sucedida a exibição para o documento será retornada. Se a autorização falhar Retornando `ChallengeResult` informará qualquer autenticação, autorização de middleware falhou e o middleware pode levar a resposta apropriada, por exemplo retornando um código de status 401 ou 403 ou redirecionar o usuário para uma página de logon para clientes de navegador interativo.
