---
title: "Autorização com base na política personalizada no núcleo do ASP.NET"
author: rick-anderson
description: "Saiba como criar e usar manipuladores de diretiva de autorização personalizada para impor requisitos de autorização em um aplicativo do ASP.NET Core."
ms.author: riande
ms.custom: mvc
manager: wpickett
ms.date: 11/21/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/policies
ms.openlocfilehash: 1067e97dd6e71021929aa3690b0c3f5bfc6c9724
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/19/2018
---
# <a name="custom-policy-based-authorization"></a><span data-ttu-id="faaad-103">Autorização personalizada com base em políticas</span><span class="sxs-lookup"><span data-stu-id="faaad-103">Custom policy-based authorization</span></span>

<span data-ttu-id="faaad-104">Nos bastidores, [autorização baseada em função](xref:security/authorization/roles) e [autorização baseada em declarações](xref:security/authorization/claims) usam um requisito, um manipulador de requisito e uma política de pré-configurada.</span><span class="sxs-lookup"><span data-stu-id="faaad-104">Underneath the covers, [role-based authorization](xref:security/authorization/roles) and [claims-based authorization](xref:security/authorization/claims) use a requirement, a requirement handler, and a pre-configured policy.</span></span> <span data-ttu-id="faaad-105">Esses blocos de construção oferecem suporte a expressão de avaliações de autorização no código.</span><span class="sxs-lookup"><span data-stu-id="faaad-105">These building blocks support the expression of authorization evaluations in code.</span></span> <span data-ttu-id="faaad-106">O resultado é uma estrutura de autorização mais sofisticados, reutilizáveis e testáveis.</span><span class="sxs-lookup"><span data-stu-id="faaad-106">The result is a richer, reusable, testable authorization structure.</span></span>

<span data-ttu-id="faaad-107">Uma política de autorização consiste em um ou mais requisitos.</span><span class="sxs-lookup"><span data-stu-id="faaad-107">An authorization policy consists of one or more requirements.</span></span> <span data-ttu-id="faaad-108">Ele é registrado como parte da configuração do serviço de autorização, no `ConfigureServices` método do `Startup` classe:</span><span class="sxs-lookup"><span data-stu-id="faaad-108">It's registered as part of the authorization service configuration, in the `ConfigureServices` method of the `Startup` class:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63,72)]

<span data-ttu-id="faaad-109">No exemplo anterior, uma política de "AtLeast21" é criada.</span><span class="sxs-lookup"><span data-stu-id="faaad-109">In the preceding example, an "AtLeast21" policy is created.</span></span> <span data-ttu-id="faaad-110">Ele tem um requisito, que de idade mínima, que é fornecido como um parâmetro para o requisito.</span><span class="sxs-lookup"><span data-stu-id="faaad-110">It has a single requirement, that of a minimum age, which is supplied as a parameter to the requirement.</span></span>

<span data-ttu-id="faaad-111">As políticas são aplicadas usando o `[Authorize]` atributo com o nome da política.</span><span class="sxs-lookup"><span data-stu-id="faaad-111">Policies are applied by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="faaad-112">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="faaad-112">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="requirements"></a><span data-ttu-id="faaad-113">Requisitos</span><span class="sxs-lookup"><span data-stu-id="faaad-113">Requirements</span></span>

<span data-ttu-id="faaad-114">Um requisito de autorização é uma coleção de parâmetros de dados que uma política pode usar para avaliar a entidade de segurança do usuário atual.</span><span class="sxs-lookup"><span data-stu-id="faaad-114">An authorization requirement is a collection of data parameters that a policy can use to evaluate the current user principal.</span></span> <span data-ttu-id="faaad-115">Em nossa política de "AtLeast21", o requisito é um único parâmetro&mdash;a idade mínima.</span><span class="sxs-lookup"><span data-stu-id="faaad-115">In our "AtLeast21" policy, the requirement is a single parameter&mdash;the minimum age.</span></span> <span data-ttu-id="faaad-116">Implementa um requisito `IAuthorizationRequirement`, que é uma interface de marcador vazio.</span><span class="sxs-lookup"><span data-stu-id="faaad-116">A requirement implements `IAuthorizationRequirement`, which is an empty marker interface.</span></span> <span data-ttu-id="faaad-117">Um requisito de idade mínima com parâmetros pode ser implementado da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="faaad-117">A parameterized minimum age requirement could be implemented as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

> [!NOTE]
> <span data-ttu-id="faaad-118">Um requisito não precisa ter dados ou propriedades.</span><span class="sxs-lookup"><span data-stu-id="faaad-118">A requirement doesn't need to have data or properties.</span></span>

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a><span data-ttu-id="faaad-119">Manipuladores de autorização</span><span class="sxs-lookup"><span data-stu-id="faaad-119">Authorization handlers</span></span>

<span data-ttu-id="faaad-120">Um manipulador de autorização é responsável pela avaliação de propriedades de um requisito.</span><span class="sxs-lookup"><span data-stu-id="faaad-120">An authorization handler is responsible for the evaluation of a requirement's properties.</span></span> <span data-ttu-id="faaad-121">O manipulador de autorização avalia os requisitos em relação a um fornecido `AuthorizationHandlerContext` para determinar se o acesso é permitido.</span><span class="sxs-lookup"><span data-stu-id="faaad-121">The authorization handler evaluates the requirements against a provided `AuthorizationHandlerContext` to determine if access is allowed.</span></span> <span data-ttu-id="faaad-122">Pode ter um requisito [vários manipuladores](#security-authorization-policies-based-multiple-handlers).</span><span class="sxs-lookup"><span data-stu-id="faaad-122">A requirement can have [multiple handlers](#security-authorization-policies-based-multiple-handlers).</span></span> <span data-ttu-id="faaad-123">Manipuladores de herdam `AuthorizationHandler<T>`, onde `T` é o requisito para ser tratada.</span><span class="sxs-lookup"><span data-stu-id="faaad-123">Handlers inherit `AuthorizationHandler<T>`, where `T` is the requirement to be handled.</span></span>

<a name="security-authorization-handler-example"></a>

<span data-ttu-id="faaad-124">O manipulador de idade mínima pode ter esta aparência:</span><span class="sxs-lookup"><span data-stu-id="faaad-124">The minimum age handler might look like this:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

<span data-ttu-id="faaad-125">O código anterior determina se a entidade de usuário atual tem uma data de nascimento de declaração que foi emitido por um emissor confiável e conhecido.</span><span class="sxs-lookup"><span data-stu-id="faaad-125">The preceding code determines if the current user principal has a date of birth claim which has been issued by a known and trusted Issuer.</span></span> <span data-ttu-id="faaad-126">A autorização não pode ocorrer quando a declaração está ausente, caso em que uma tarefa concluída é retornada.</span><span class="sxs-lookup"><span data-stu-id="faaad-126">Authorization can't occur when the claim is missing, in which case a completed task is returned.</span></span> <span data-ttu-id="faaad-127">Quando uma declaração estiver presente, a duração do usuário é calculada.</span><span class="sxs-lookup"><span data-stu-id="faaad-127">When a claim is present, the user's age is calculated.</span></span> <span data-ttu-id="faaad-128">Se o usuário atende a idade mínima definida pelo requisito de autorização é considerada bem-sucedido.</span><span class="sxs-lookup"><span data-stu-id="faaad-128">If the user meets the minimum age defined by the requirement, authorization is deemed successful.</span></span> <span data-ttu-id="faaad-129">Quando a autorização for bem-sucedido, `context.Succeed` é invocado com o requisito satisfeito como um parâmetro.</span><span class="sxs-lookup"><span data-stu-id="faaad-129">When authorization is successful, `context.Succeed` is invoked with the satisfied requirement as a parameter.</span></span>

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a><span data-ttu-id="faaad-130">Registro do manipulador</span><span class="sxs-lookup"><span data-stu-id="faaad-130">Handler registration</span></span>

<span data-ttu-id="faaad-131">Manipuladores são registrados na coleção de serviços durante a configuração.</span><span class="sxs-lookup"><span data-stu-id="faaad-131">Handlers are registered in the services collection during configuration.</span></span> <span data-ttu-id="faaad-132">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="faaad-132">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63-65,72)]

<span data-ttu-id="faaad-133">Cada manipulador é adicionado à coleção de serviços invocando `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();`.</span><span class="sxs-lookup"><span data-stu-id="faaad-133">Each handler is added to the services collection by invoking `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();`.</span></span>

## <a name="what-should-a-handler-return"></a><span data-ttu-id="faaad-134">O que deve retornar um manipulador?</span><span class="sxs-lookup"><span data-stu-id="faaad-134">What should a handler return?</span></span>

<span data-ttu-id="faaad-135">Observe que o `Handle` método o [exemplo manipulador](#security-authorization-handler-example) não retorna nenhum valor.</span><span class="sxs-lookup"><span data-stu-id="faaad-135">Note that the `Handle` method in the [handler example](#security-authorization-handler-example) returns no value.</span></span> <span data-ttu-id="faaad-136">Como é um status de êxito ou falha indicado?</span><span class="sxs-lookup"><span data-stu-id="faaad-136">How is a status of either success or failure indicated?</span></span>

* <span data-ttu-id="faaad-137">Um manipulador indica êxito chamando `context.Succeed(IAuthorizationRequirement requirement)`, passando o requisito de que foi validada com êxito.</span><span class="sxs-lookup"><span data-stu-id="faaad-137">A handler indicates success by calling `context.Succeed(IAuthorizationRequirement requirement)`, passing the requirement that has been successfully validated.</span></span>

* <span data-ttu-id="faaad-138">Um manipulador não precisa lidar com falhas em geral, como outros manipuladores para o mesmo requisito podem ser bem-sucedida.</span><span class="sxs-lookup"><span data-stu-id="faaad-138">A handler does not need to handle failures generally, as other handlers for the same requirement may succeed.</span></span>

* <span data-ttu-id="faaad-139">Para garantir a falha, mesmo se outros manipuladores de requisito tenha êxito, chame `context.Fail`.</span><span class="sxs-lookup"><span data-stu-id="faaad-139">To guarantee failure, even if other requirement handlers succeed, call `context.Fail`.</span></span>

<span data-ttu-id="faaad-140">Todos os manipuladores para um requisito, independentemente do que você chamar dentro de seu manipulador, serão chamados quando uma política requer que o requisito.</span><span class="sxs-lookup"><span data-stu-id="faaad-140">Regardless of what you call inside your handler, all handlers for a requirement will be called when a policy requires the requirement.</span></span> <span data-ttu-id="faaad-141">Isso permite que os requisitos para ter efeitos colaterais, como o registro, que sempre ocorrerá mesmo se `context.Fail()` foi chamado no manipulador de outro.</span><span class="sxs-lookup"><span data-stu-id="faaad-141">This allows requirements to have side effects, such as logging, which will always take place even if `context.Fail()` has been called in another handler.</span></span>

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a><span data-ttu-id="faaad-142">Por que eu quero vários manipuladores para um requisito?</span><span class="sxs-lookup"><span data-stu-id="faaad-142">Why would I want multiple handlers for a requirement?</span></span>

<span data-ttu-id="faaad-143">Em casos onde você deseja evaluation para estar em um **ou** base, implementar vários manipuladores para um requisito.</span><span class="sxs-lookup"><span data-stu-id="faaad-143">In cases where you want evaluation to be on an **OR** basis, implement multiple handlers for a single requirement.</span></span> <span data-ttu-id="faaad-144">Por exemplo, a Microsoft tem portas que apenas abrir com cartões de chave.</span><span class="sxs-lookup"><span data-stu-id="faaad-144">For example, Microsoft has doors which only open with key cards.</span></span> <span data-ttu-id="faaad-145">Se você deixar o seu cartão de chave em casa, o recepcionista imprime uma etiqueta temporária e abre a porta para você.</span><span class="sxs-lookup"><span data-stu-id="faaad-145">If you leave your key card at home, the receptionist prints a temporary sticker and opens the door for you.</span></span> <span data-ttu-id="faaad-146">Nesse cenário, você teria um requisito, *BuildingEntry*, mas vários manipuladores, cada um deles examinando um requisito.</span><span class="sxs-lookup"><span data-stu-id="faaad-146">In this scenario, you'd have a single requirement, *BuildingEntry*, but multiple handlers, each one examining a single requirement.</span></span>

<span data-ttu-id="faaad-147">*BuildingEntryRequirement.cs*</span><span class="sxs-lookup"><span data-stu-id="faaad-147">*BuildingEntryRequirement.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

<span data-ttu-id="faaad-148">*BadgeEntryHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="faaad-148">*BadgeEntryHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

<span data-ttu-id="faaad-149">*TemporaryStickerHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="faaad-149">*TemporaryStickerHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

<span data-ttu-id="faaad-150">Certifique-se de que ambos os manipuladores são [registrado](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span><span class="sxs-lookup"><span data-stu-id="faaad-150">Ensure that both handlers are [registered](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span></span> <span data-ttu-id="faaad-151">Se o manipulador é bem-sucedida quando uma política avalia o `BuildingEntryRequirement`, a avaliação de política for bem-sucedida.</span><span class="sxs-lookup"><span data-stu-id="faaad-151">If either handler succeeds when a policy evaluates the `BuildingEntryRequirement`, the policy evaluation succeeds.</span></span>

## <a name="using-a-func-to-fulfill-a-policy"></a><span data-ttu-id="faaad-152">Usando uma função para atender a uma política</span><span class="sxs-lookup"><span data-stu-id="faaad-152">Using a func to fulfill a policy</span></span>

<span data-ttu-id="faaad-153">Pode haver situações nas quais que atendem a uma política é simple de expressar em código.</span><span class="sxs-lookup"><span data-stu-id="faaad-153">There may be situations in which fulfilling a policy is simple to express in code.</span></span> <span data-ttu-id="faaad-154">É possível fornecer uma `Func<AuthorizationHandlerContext, bool>` ao configurar a política com o `RequireAssertion` criador de políticas.</span><span class="sxs-lookup"><span data-stu-id="faaad-154">It's possible to supply a `Func<AuthorizationHandlerContext, bool>` when configuring your policy with the `RequireAssertion` policy builder.</span></span>

<span data-ttu-id="faaad-155">Por exemplo, o anterior `BadgeEntryHandler` poderia ser reescrito da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="faaad-155">For example, the previous `BadgeEntryHandler` could be rewritten as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=52-53,57-63)]

## <a name="accessing-mvc-request-context-in-handlers"></a><span data-ttu-id="faaad-156">Acessando o contexto de solicitação do MVC em manipuladores</span><span class="sxs-lookup"><span data-stu-id="faaad-156">Accessing MVC request context in handlers</span></span>

<span data-ttu-id="faaad-157">O `HandleRequirementAsync` método implementar um manipulador de autorização tem dois parâmetros: um `AuthorizationHandlerContext` e o `TRequirement` manipulada.</span><span class="sxs-lookup"><span data-stu-id="faaad-157">The `HandleRequirementAsync` method you implement in an authorization handler has two parameters: an `AuthorizationHandlerContext` and the `TRequirement` you are handling.</span></span> <span data-ttu-id="faaad-158">Estruturas como MVC ou Jabbr serão livres para adicionar qualquer objeto para o `Resource` propriedade o `AuthorizationHandlerContext` para passar informações adicionais.</span><span class="sxs-lookup"><span data-stu-id="faaad-158">Frameworks such as MVC or Jabbr are free to add any object to the `Resource` property on the `AuthorizationHandlerContext` to pass extra information.</span></span>

<span data-ttu-id="faaad-159">Por exemplo, o MVC passa uma instância de [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) no `Resource` propriedade.</span><span class="sxs-lookup"><span data-stu-id="faaad-159">For example, MVC passes an instance of [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) in the `Resource` property.</span></span> <span data-ttu-id="faaad-160">Esta propriedade fornece acesso a `HttpContext`, `RouteData`e tudo pessoa forneceu MVC e páginas Razor.</span><span class="sxs-lookup"><span data-stu-id="faaad-160">This property provides access to `HttpContext`, `RouteData`, and everything else provided by MVC and Razor Pages.</span></span>

<span data-ttu-id="faaad-161">O uso do `Resource` é de propriedade específicos da estrutura.</span><span class="sxs-lookup"><span data-stu-id="faaad-161">The use of the `Resource` property is framework specific.</span></span> <span data-ttu-id="faaad-162">Usando informações de `Resource` propriedade limita suas políticas de autorização para estruturas específicas.</span><span class="sxs-lookup"><span data-stu-id="faaad-162">Using information in the `Resource` property limits your authorization policies to particular frameworks.</span></span> <span data-ttu-id="faaad-163">Você deve converter o `Resource` propriedade usando o `as` palavra-chave e, em seguida, confirme a conversão tenha êxito para garantir que seu código não falha com um `InvalidCastException` quando executado em outras estruturas:</span><span class="sxs-lookup"><span data-stu-id="faaad-163">You should cast the `Resource` property using the `as` keyword, and then confirm the cast has succeed to ensure your code doesn't crash with an `InvalidCastException` when run on other frameworks:</span></span>

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```
