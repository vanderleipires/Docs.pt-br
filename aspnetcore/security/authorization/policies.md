---
title: "Autorização com base na política personalizada no núcleo do ASP.NET"
author: rick-anderson
description: "Saiba como criar e usar manipuladores de diretiva de autorização personalizada para impor requisitos de autorização em um aplicativo do ASP.NET Core."
keywords: "ASP.NET Core, autorização, a política personalizada, a política de autorização"
ms.author: riande
ms.custom: mvc
manager: wpickett
ms.date: 11/21/2017
ms.topic: article
ms.assetid: e422a1b2-dc4a-4bcc-b8d9-7ee62009b6a3
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/policies
ms.openlocfilehash: 280dd72b75e39546061d8455931f597f50c829fe
ms.sourcegitcommit: f1436107b4c022b26f5235dddef103cec5aa6bff
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/15/2017
---
# <a name="custom-policy-based-authorization"></a>Autorização personalizada com base em políticas

Nos bastidores, [autorização baseada em função](xref:security/authorization/roles) e [autorização baseada em declarações](xref:security/authorization/claims) usam um requisito, um manipulador de requisito e uma política de pré-configurada. Esses blocos de construção oferecem suporte a expressão de avaliações de autorização no código. O resultado é uma estrutura de autorização mais sofisticados, reutilizáveis e testáveis.

Uma política de autorização consiste em um ou mais requisitos. Ele é registrado como parte da configuração do serviço de autorização, no `ConfigureServices` método do `Startup` classe:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63,72)]

No exemplo anterior, uma política de "AtLeast21" é criada. Ele tem um requisito, que de idade mínima, que é fornecido como um parâmetro para o requisito.

As políticas são aplicadas usando o `[Authorize]` atributo com o nome da política. Por exemplo:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="requirements"></a>Requisitos

Um requisito de autorização é uma coleção de parâmetros de dados que uma política pode usar para avaliar a entidade de segurança do usuário atual. Em nossa política de "AtLeast21", o requisito é um único parâmetro&mdash;a idade mínima. Implementa um requisito `IAuthorizationRequirement`, que é uma interface de marcador vazio. Um requisito de idade mínima com parâmetros pode ser implementado da seguinte maneira:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

> [!NOTE]
> Um requisito não precisa ter dados ou propriedades.

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a>Manipuladores de autorização

Um manipulador de autorização é responsável pela avaliação de propriedades de um requisito. O manipulador de autorização avalia os requisitos em relação a um fornecido `AuthorizationHandlerContext` para determinar se o acesso é permitido. Pode ter um requisito [vários manipuladores](#security-authorization-policies-based-multiple-handlers). Manipuladores de herdam `AuthorizationHandler<T>`, onde `T` é o requisito para ser tratada.

<a name="security-authorization-handler-example"></a>

O manipulador de idade mínima pode ter esta aparência:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

O código anterior determina se a entidade de usuário atual tem uma data de nascimento de declaração que foi emitido por um emissor confiável e conhecido. A autorização não pode ocorrer quando a declaração está ausente, caso em que uma tarefa concluída é retornada. Quando uma declaração estiver presente, a duração do usuário é calculada. Se o usuário atende a idade mínima definida pelo requisito de autorização é considerada bem-sucedido. Quando a autorização for bem-sucedido, `context.Succeed` é invocado com o requisito satisfeito como um parâmetro.

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a>Registro do manipulador

Manipuladores são registrados na coleção de serviços durante a configuração. Por exemplo:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63-65,72)]

Cada manipulador é adicionado à coleção de serviços invocando `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();`.

## <a name="what-should-a-handler-return"></a>O que deve retornar um manipulador?

Observe que o `Handle` método o [exemplo manipulador](#security-authorization-handler-example) não retorna nenhum valor. Como é um status de êxito ou falha indicado?

* Um manipulador indica êxito chamando `context.Succeed(IAuthorizationRequirement requirement)`, passando o requisito de que foi validada com êxito.

* Um manipulador não precisa lidar com falhas em geral, como outros manipuladores para o mesmo requisito podem ser bem-sucedida.

* Para garantir a falha, mesmo se outros manipuladores de requisito tenha êxito, chame `context.Fail`.

Todos os manipuladores para um requisito, independentemente do que você chamar dentro de seu manipulador, serão chamados quando uma política requer que o requisito. Isso permite que os requisitos para ter efeitos colaterais, como o registro, que sempre ocorrerá mesmo se `context.Fail()` foi chamado no manipulador de outro.

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a>Por que eu quero vários manipuladores para um requisito?

Em casos onde você deseja evaluation para estar em um **ou** base, implementar vários manipuladores para um requisito. Por exemplo, a Microsoft tem portas que apenas abrir com cartões de chave. Se você deixar o seu cartão de chave em casa, o recepcionista imprime uma etiqueta temporária e abre a porta para você. Nesse cenário, você teria um requisito, *BuildingEntry*, mas vários manipuladores, cada um deles examinando um requisito.

*BuildingEntryRequirement.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

*BadgeEntryHandler.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

*TemporaryStickerHandler.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

Certifique-se de que ambos os manipuladores são [registrado](xref:security/authorization/policies#security-authorization-policies-based-handler-registration). Se o manipulador é bem-sucedida quando uma política avalia o `BuildingEntryRequirement`, a avaliação de política for bem-sucedida.

## <a name="using-a-func-to-fulfill-a-policy"></a>Usando uma função para atender a uma política

Pode haver situações nas quais que atendem a uma política é simple de expressar em código. É possível fornecer uma `Func<AuthorizationHandlerContext, bool>` ao configurar a política com o `RequireAssertion` criador de políticas.

Por exemplo, o anterior `BadgeEntryHandler` poderia ser reescrito da seguinte maneira:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=52-53,57-63)]

## <a name="accessing-mvc-request-context-in-handlers"></a>Acessando o contexto de solicitação do MVC em manipuladores

O `HandleRequirementAsync` método implementar um manipulador de autorização tem dois parâmetros: um `AuthorizationHandlerContext` e o `TRequirement` manipulada. Estruturas como MVC ou Jabbr serão livres para adicionar qualquer objeto para o `Resource` propriedade o `AuthorizationHandlerContext` para passar informações adicionais.

Por exemplo, o MVC passa uma instância de [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) no `Resource` propriedade. Esta propriedade fornece acesso a `HttpContext`, `RouteData`e tudo pessoa forneceu MVC e páginas Razor.

O uso do `Resource` é de propriedade específicos da estrutura. Usando informações de `Resource` propriedade limita suas políticas de autorização para estruturas específicas. Você deve converter o `Resource` propriedade usando o `as` palavra-chave e, em seguida, confirme a conversão tenha êxito para garantir que seu código não falha com um `InvalidCastException` quando executado em outras estruturas:

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```
