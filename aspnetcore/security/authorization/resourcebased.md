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
ms.openlocfilehash: 3be2d9b9aef18763fbdba78e044dd6b68ddcef0c
ms.sourcegitcommit: 9bc34b8269d2a150b844c3b8646dcb30278a95ea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/12/2018
---
# <a name="resource-based-authorization-in-aspnet-core"></a>Autorização baseada em recursos no ASP.NET Core

Estratégia de autorização depende do recurso que está sendo acessado. Considere a possibilidade de um documento que tem uma propriedade de autor. Somente o autor tem permissão para atualizar o documento. Consequentemente, o documento deve ser recuperado do armazenamento de dados antes de avaliação de autorização pode ocorrer.

Avaliação do atributo ocorre antes da associação de dados e antes da execução do manipulador de página ou ação que carrega o documento. Por esses motivos, autorização declarativa com um `[Authorize]` atributo não é suficiente. Em vez disso, você pode chamar um método de autorização personalizada&mdash;um estilo conhecido como autorização obrigatória.

Use o [aplicativos de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples) ([como baixar](xref:tutorials/index#how-to-download-a-sample)) para explorar os recursos descritos neste tópico.

[Criar um aplicativo do ASP.NET Core com dados de usuário protegidos por autorização](xref:security/authorization/secure-data) contém um aplicativo de exemplo que usa a autorização baseada em recursos.

## <a name="use-imperative-authorization"></a>Usar autorização obrigatória

Autorização é implementada como um [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) de serviço e é registrada na coleção de serviço dentro de `Startup` classe. O serviço é disponibilizado por meio de [injeção de dependência](xref:fundamentals/dependency-injection#fundamentals-dependency-injection) manipuladores de página ou ações.

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Controllers/DocumentController.cs?name=snippet_IAuthServiceDI&highlight=6)]

`IAuthorizationService` tem duas `AuthorizeAsync` sobrecargas do método: aceitando um recurso e o nome da política e o outro aceitando o recurso e uma lista de requisitos para avaliar.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          IEnumerable<IAuthorizationRequirement> requirements);
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          string policyName);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

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

O exemplo a seguir, o recurso a ser protegida é carregado em um personalizado `Document` objeto. Um `AuthorizeAsync` sobrecarga é chamada para determinar se o usuário atual tem permissão para editar o documento. Uma diretiva de autorização personalizada "EditPolicy" é acrescentada a uma decisão. Consulte [autorização com base na política de personalizado](xref:security/authorization/policies) para obter mais informações sobre a criação de políticas de autorização.

> [!NOTE]
> O código a seguir exemplos pressupõem a autenticação foi executado e o conjunto de `User` propriedade.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/Edit.cshtml.cs?name=snippet_DocumentEditHandler)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentEditAction)]

---

## <a name="write-a-resource-based-handler"></a>Escrever um Gerenciador de recursos

Escrevendo um manipulador para a autorização baseada em recursos não é muito diferente de [escrevendo um manipulador de requisitos simples](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler). Criar uma classe personalizada de requisito e implementar uma classe de manipulador de requisito. A classe do manipulador Especifica o requisito e o tipo de recurso. Por exemplo, um uso de manipulador um `SameAuthorRequirement` requisito e um `Document` recurso tem a seguinte aparência:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

---

Registrar o manipulador no e o requisito de `Startup.ConfigureServices` método:

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Startup.cs?name=snippet_ConfigureServicesSample&highlight=3-7,9)]

### <a name="operational-requirements"></a>Requisitos operacionais

Se você estiver fazendo decisões com base nos resultados das operações CRUD (criar, ler, atualizar, excluir), use o [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) classe auxiliar. Essa classe permite que você grave um único manipulador em vez de uma classe individual para cada tipo de operação. Para usá-lo, forneça alguns nomes de operação:

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_OperationsClass)]

O manipulador é implementado como a seguir, usando um `OperationAuthorizationRequirement` requisito e um `Document` recursos:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

---

O manipulador anterior valida a operação usando o recurso, a identidade do usuário e o requisito `Name` propriedade.

Para chamar um manipulador de recurso operacionais, especifique a operação ao invocar `AuthorizeAsync` no manipulador de página ou ação. O exemplo a seguir determina se o usuário autenticado tem permissão para exibir o documento.

> [!NOTE]
> O código a seguir exemplos pressupõem a autenticação foi executado e o conjunto de `User` propriedade.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/View.cshtml.cs?name=snippet_DocumentViewHandler&highlight=10-11)]

Se a autorização bem-sucedida, a página para exibir o documento será retornada. Se falhar de autorização, mas o usuário é autenticado, o retorno `ForbidResult` informa qualquer middleware de autenticação com falha de autorização. Um `ChallengeResult` é retornado quando a autenticação deve ser executada. Para clientes de navegador interativo, pode ser apropriado redirecionar o usuário para uma página de logon.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentViewAction&highlight=11-12)]

Se a autorização bem-sucedida, o modo de exibição para o documento será retornado. Se a autorização falhar, retornando `ChallengeResult` informa qualquer middleware de autenticação que a falha de autorização e o middleware pode levar a resposta apropriada. Uma resposta apropriada pode estar retornando um código de status 401 ou 403. Para clientes de navegador interativo, isso poderia significar redirecionar o usuário para uma página de logon.

---
