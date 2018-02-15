---
title: "Autorização baseada em política no ASP.NET Core"
author: rick-anderson
description: "Saiba como criar e usar manipuladores de diretiva de autorização para impor requisitos de autorização em um aplicativo do ASP.NET Core."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 11/21/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/policies
ms.openlocfilehash: a9ee7e6fd06fa88485d7f578a9df74cbf87d9540
ms.sourcegitcommit: 7ee6e7582421195cbd675355c970d3d292ee668d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/14/2018
---
# <a name="policy-based-authorization"></a><span data-ttu-id="9c9f8-103">Autorização baseada em política</span><span class="sxs-lookup"><span data-stu-id="9c9f8-103">Policy-based authorization</span></span>

<span data-ttu-id="9c9f8-104">Nos bastidores, [autorização baseada em função](xref:security/authorization/roles) e [autorização baseada em declarações](xref:security/authorization/claims) usam um requisito, um manipulador de requisito e uma política de pré-configurada.</span><span class="sxs-lookup"><span data-stu-id="9c9f8-104">Underneath the covers, [role-based authorization](xref:security/authorization/roles) and [claims-based authorization](xref:security/authorization/claims) use a requirement, a requirement handler, and a pre-configured policy.</span></span> <span data-ttu-id="9c9f8-105">Esses blocos de construção oferecem suporte a expressão de avaliações de autorização no código.</span><span class="sxs-lookup"><span data-stu-id="9c9f8-105">These building blocks support the expression of authorization evaluations in code.</span></span> <span data-ttu-id="9c9f8-106">O resultado é uma estrutura de autorização mais sofisticados, reutilizáveis e testáveis.</span><span class="sxs-lookup"><span data-stu-id="9c9f8-106">The result is a richer, reusable, testable authorization structure.</span></span>

<span data-ttu-id="9c9f8-107">Uma política de autorização consiste em um ou mais requisitos.</span><span class="sxs-lookup"><span data-stu-id="9c9f8-107">An authorization policy consists of one or more requirements.</span></span> <span data-ttu-id="9c9f8-108">Ele é registrado como parte da configuração do serviço de autorização no `Startup.ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="9c9f8-108">It's registered as part of the authorization service configuration, in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63,72)]

<span data-ttu-id="9c9f8-109">No exemplo anterior, uma política de "AtLeast21" é criada.</span><span class="sxs-lookup"><span data-stu-id="9c9f8-109">In the preceding example, an "AtLeast21" policy is created.</span></span> <span data-ttu-id="9c9f8-110">Ele tem um requisito&mdash;de idade mínima, que é fornecida como um parâmetro para o requisito.</span><span class="sxs-lookup"><span data-stu-id="9c9f8-110">It has a single requirement&mdash;that of a minimum age, which is supplied as a parameter to the requirement.</span></span>

<span data-ttu-id="9c9f8-111">As políticas são aplicadas usando o `[Authorize]` atributo com o nome da política.</span><span class="sxs-lookup"><span data-stu-id="9c9f8-111">Policies are applied by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="9c9f8-112">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="9c9f8-112">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="requirements"></a><span data-ttu-id="9c9f8-113">Requisitos</span><span class="sxs-lookup"><span data-stu-id="9c9f8-113">Requirements</span></span>

<span data-ttu-id="9c9f8-114">Um requisito de autorização é uma coleção de parâmetros de dados que uma política pode usar para avaliar a entidade de segurança do usuário atual.</span><span class="sxs-lookup"><span data-stu-id="9c9f8-114">An authorization requirement is a collection of data parameters that a policy can use to evaluate the current user principal.</span></span> <span data-ttu-id="9c9f8-115">Em nossa política de "AtLeast21", o requisito é um único parâmetro&mdash;a idade mínima.</span><span class="sxs-lookup"><span data-stu-id="9c9f8-115">In our "AtLeast21" policy, the requirement is a single parameter&mdash;the minimum age.</span></span> <span data-ttu-id="9c9f8-116">Implementa um requisito [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), que é uma interface de marcador vazio.</span><span class="sxs-lookup"><span data-stu-id="9c9f8-116">A requirement implements [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), which is an empty marker interface.</span></span> <span data-ttu-id="9c9f8-117">Um requisito de idade mínima com parâmetros pode ser implementado da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="9c9f8-117">A parameterized minimum age requirement could be implemented as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

> [!NOTE]
> <span data-ttu-id="9c9f8-118">Um requisito não precisa ter dados ou propriedades.</span><span class="sxs-lookup"><span data-stu-id="9c9f8-118">A requirement doesn't need to have data or properties.</span></span>

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a><span data-ttu-id="9c9f8-119">Manipuladores de autorização</span><span class="sxs-lookup"><span data-stu-id="9c9f8-119">Authorization handlers</span></span>

<span data-ttu-id="9c9f8-120">Um manipulador de autorização é responsável pela avaliação de propriedades de um requisito.</span><span class="sxs-lookup"><span data-stu-id="9c9f8-120">An authorization handler is responsible for the evaluation of a requirement's properties.</span></span> <span data-ttu-id="9c9f8-121">O manipulador de autorização avalia os requisitos em relação a um fornecido [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) para determinar se o acesso é permitido.</span><span class="sxs-lookup"><span data-stu-id="9c9f8-121">The authorization handler evaluates the requirements against a provided [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) to determine if access is allowed.</span></span>

<span data-ttu-id="9c9f8-122">Pode ter um requisito [vários manipuladores](#security-authorization-policies-based-multiple-handlers).</span><span class="sxs-lookup"><span data-stu-id="9c9f8-122">A requirement can have [multiple handlers](#security-authorization-policies-based-multiple-handlers).</span></span> <span data-ttu-id="9c9f8-123">Um manipulador pode herdar [AuthorizationHandler\<TRequirement >](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), onde `TRequirement` é o requisito para ser tratada.</span><span class="sxs-lookup"><span data-stu-id="9c9f8-123">A handler may inherit [AuthorizationHandler\<TRequirement>](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), where `TRequirement` is the requirement to be handled.</span></span> <span data-ttu-id="9c9f8-124">Como alternativa, um manipulador pode implementar [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) para lidar com mais de um tipo de requisito.</span><span class="sxs-lookup"><span data-stu-id="9c9f8-124">Alternatively, a handler may implement [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) to handle more than one type of requirement.</span></span>

### <a name="use-a-handler-for-one-requirement"></a><span data-ttu-id="9c9f8-125">Usar um manipulador de um requisito</span><span class="sxs-lookup"><span data-stu-id="9c9f8-125">Use a handler for one requirement</span></span>

<a name="security-authorization-handler-example"></a>

<span data-ttu-id="9c9f8-126">Este é um exemplo de uma relação um para um no qual um manipulador de idade mínima utiliza um requisito:</span><span class="sxs-lookup"><span data-stu-id="9c9f8-126">The following is an example of a one-to-one relationship in which a minimum age handler utilizes a single requirement:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

<span data-ttu-id="9c9f8-127">O código anterior determina se a entidade de usuário atual tem uma data de nascimento de declaração que foi emitido por um emissor confiável e conhecido.</span><span class="sxs-lookup"><span data-stu-id="9c9f8-127">The preceding code determines if the current user principal has a date of birth claim which has been issued by a known and trusted Issuer.</span></span> <span data-ttu-id="9c9f8-128">A autorização não pode ocorrer quando a declaração está ausente, caso em que uma tarefa concluída é retornada.</span><span class="sxs-lookup"><span data-stu-id="9c9f8-128">Authorization can't occur when the claim is missing, in which case a completed task is returned.</span></span> <span data-ttu-id="9c9f8-129">Quando uma declaração estiver presente, a duração do usuário é calculada.</span><span class="sxs-lookup"><span data-stu-id="9c9f8-129">When a claim is present, the user's age is calculated.</span></span> <span data-ttu-id="9c9f8-130">Se o usuário atende a idade mínima definida pelo requisito de autorização é considerada bem-sucedido.</span><span class="sxs-lookup"><span data-stu-id="9c9f8-130">If the user meets the minimum age defined by the requirement, authorization is deemed successful.</span></span> <span data-ttu-id="9c9f8-131">Quando a autorização for bem-sucedido, `context.Succeed` é invocado com o requisito satisfeito como seu único parâmetro.</span><span class="sxs-lookup"><span data-stu-id="9c9f8-131">When authorization is successful, `context.Succeed` is invoked with the satisfied requirement as its sole parameter.</span></span>

### <a name="use-a-handler-for-multiple-requirements"></a><span data-ttu-id="9c9f8-132">Usar um manipulador para várias necessidades</span><span class="sxs-lookup"><span data-stu-id="9c9f8-132">Use a handler for multiple requirements</span></span>

<span data-ttu-id="9c9f8-133">Este é um exemplo de uma relação um-para-muitos em que um manipulador de permissão utiliza três requisitos:</span><span class="sxs-lookup"><span data-stu-id="9c9f8-133">The following is an example of a one-to-many relationship in which a permission handler utilizes three requirements:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/PermissionHandler.cs?name=snippet_PermissionHandlerClass)]

<span data-ttu-id="9c9f8-134">O código anterior atravessa [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;uma propriedade que contém requisitos não marcada como bem-sucedido.</span><span class="sxs-lookup"><span data-stu-id="9c9f8-134">The preceding code traverses [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;a property containing requirements not marked as successful.</span></span> <span data-ttu-id="9c9f8-135">Se o usuário tem permissão de leitura, ele deve ser um proprietário ou um patrocinador para acessar o recurso solicitado.</span><span class="sxs-lookup"><span data-stu-id="9c9f8-135">If the user has read permission, he or she must be either an owner or a sponsor to access the requested resource.</span></span> <span data-ttu-id="9c9f8-136">Se o usuário editar ou excluir permissão, deve ser um proprietário para acessar o recurso solicitado.</span><span class="sxs-lookup"><span data-stu-id="9c9f8-136">If the user has edit or delete permission, he or she must be an owner to access the requested resource.</span></span> <span data-ttu-id="9c9f8-137">Quando a autorização for bem-sucedido, `context.Succeed` é invocado com o requisito satisfeito como seu único parâmetro.</span><span class="sxs-lookup"><span data-stu-id="9c9f8-137">When authorization is successful, `context.Succeed` is invoked with the satisfied requirement as its sole parameter.</span></span>

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a><span data-ttu-id="9c9f8-138">Registro do manipulador</span><span class="sxs-lookup"><span data-stu-id="9c9f8-138">Handler registration</span></span>

<span data-ttu-id="9c9f8-139">Manipuladores são registrados na coleção de serviços durante a configuração.</span><span class="sxs-lookup"><span data-stu-id="9c9f8-139">Handlers are registered in the services collection during configuration.</span></span> <span data-ttu-id="9c9f8-140">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="9c9f8-140">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63-65,72)]

<span data-ttu-id="9c9f8-141">Cada manipulador é adicionado à coleção de serviços invocando `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();`.</span><span class="sxs-lookup"><span data-stu-id="9c9f8-141">Each handler is added to the services collection by invoking `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();`.</span></span>

## <a name="what-should-a-handler-return"></a><span data-ttu-id="9c9f8-142">O que deve retornar um manipulador?</span><span class="sxs-lookup"><span data-stu-id="9c9f8-142">What should a handler return?</span></span>

<span data-ttu-id="9c9f8-143">Observe que o `Handle` método o [exemplo manipulador](#security-authorization-handler-example) não retorna nenhum valor.</span><span class="sxs-lookup"><span data-stu-id="9c9f8-143">Note that the `Handle` method in the [handler example](#security-authorization-handler-example) returns no value.</span></span> <span data-ttu-id="9c9f8-144">Como é um status de êxito ou falha indicado?</span><span class="sxs-lookup"><span data-stu-id="9c9f8-144">How is a status of either success or failure indicated?</span></span>

* <span data-ttu-id="9c9f8-145">Um manipulador indica êxito chamando `context.Succeed(IAuthorizationRequirement requirement)`, passando o requisito de que foi validada com êxito.</span><span class="sxs-lookup"><span data-stu-id="9c9f8-145">A handler indicates success by calling `context.Succeed(IAuthorizationRequirement requirement)`, passing the requirement that has been successfully validated.</span></span>

* <span data-ttu-id="9c9f8-146">Um manipulador não precisa lidar com falhas em geral, como outros manipuladores para o mesmo requisito podem ser bem-sucedida.</span><span class="sxs-lookup"><span data-stu-id="9c9f8-146">A handler doesn't need to handle failures generally, as other handlers for the same requirement may succeed.</span></span>

* <span data-ttu-id="9c9f8-147">Para garantir a falha, mesmo se outros manipuladores de requisito tenha êxito, chame `context.Fail`.</span><span class="sxs-lookup"><span data-stu-id="9c9f8-147">To guarantee failure, even if other requirement handlers succeed, call `context.Fail`.</span></span>

<span data-ttu-id="9c9f8-148">Quando definido como `false`, o [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) propriedade (disponível no ASP.NET Core 1.1 e posterior) reduz a execução de manipuladores quando `context.Fail` é chamado.</span><span class="sxs-lookup"><span data-stu-id="9c9f8-148">When set to `false`, the [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) property (available in ASP.NET Core 1.1 and later) short-circuits the execution of handlers when `context.Fail` is called.</span></span> <span data-ttu-id="9c9f8-149">`InvokeHandlersAfterFailure` o padrão será a `true`, caso em que todos os manipuladores são chamados.</span><span class="sxs-lookup"><span data-stu-id="9c9f8-149">`InvokeHandlersAfterFailure` defaults to `true`, in which case all handlers are called.</span></span> <span data-ttu-id="9c9f8-150">Isso permite que os requisitos produzir efeitos colaterais, como o registro em log, que sempre ocorrem mesmo se `context.Fail` foi chamado no manipulador de outro.</span><span class="sxs-lookup"><span data-stu-id="9c9f8-150">This allows requirements to produce side effects, such as logging, which always take place even if `context.Fail` has been called in another handler.</span></span>

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a><span data-ttu-id="9c9f8-151">Por que eu quero vários manipuladores para um requisito?</span><span class="sxs-lookup"><span data-stu-id="9c9f8-151">Why would I want multiple handlers for a requirement?</span></span>

<span data-ttu-id="9c9f8-152">Em casos onde você deseja evaluation para estar em um **ou** base, implementar vários manipuladores para um requisito.</span><span class="sxs-lookup"><span data-stu-id="9c9f8-152">In cases where you want evaluation to be on an **OR** basis, implement multiple handlers for a single requirement.</span></span> <span data-ttu-id="9c9f8-153">Por exemplo, a Microsoft tem portas que apenas abrir com cartões de chave.</span><span class="sxs-lookup"><span data-stu-id="9c9f8-153">For example, Microsoft has doors which only open with key cards.</span></span> <span data-ttu-id="9c9f8-154">Se você deixar o seu cartão de chave em casa, o recepcionista imprime uma etiqueta temporária e abre a porta para você.</span><span class="sxs-lookup"><span data-stu-id="9c9f8-154">If you leave your key card at home, the receptionist prints a temporary sticker and opens the door for you.</span></span> <span data-ttu-id="9c9f8-155">Nesse cenário, você teria um requisito, *BuildingEntry*, mas vários manipuladores, cada um deles examinando um requisito.</span><span class="sxs-lookup"><span data-stu-id="9c9f8-155">In this scenario, you'd have a single requirement, *BuildingEntry*, but multiple handlers, each one examining a single requirement.</span></span>

<span data-ttu-id="9c9f8-156">*BuildingEntryRequirement.cs*</span><span class="sxs-lookup"><span data-stu-id="9c9f8-156">*BuildingEntryRequirement.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

<span data-ttu-id="9c9f8-157">*BadgeEntryHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="9c9f8-157">*BadgeEntryHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

<span data-ttu-id="9c9f8-158">*TemporaryStickerHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="9c9f8-158">*TemporaryStickerHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

<span data-ttu-id="9c9f8-159">Certifique-se de que ambos os manipuladores são [registrado](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span><span class="sxs-lookup"><span data-stu-id="9c9f8-159">Ensure that both handlers are [registered](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span></span> <span data-ttu-id="9c9f8-160">Se o manipulador é bem-sucedida quando uma política avalia o `BuildingEntryRequirement`, a avaliação de política for bem-sucedida.</span><span class="sxs-lookup"><span data-stu-id="9c9f8-160">If either handler succeeds when a policy evaluates the `BuildingEntryRequirement`, the policy evaluation succeeds.</span></span>

## <a name="using-a-func-to-fulfill-a-policy"></a><span data-ttu-id="9c9f8-161">Usando uma função para atender a uma política</span><span class="sxs-lookup"><span data-stu-id="9c9f8-161">Using a func to fulfill a policy</span></span>

<span data-ttu-id="9c9f8-162">Pode haver situações nas quais que atendem a uma política é simple de expressar em código.</span><span class="sxs-lookup"><span data-stu-id="9c9f8-162">There may be situations in which fulfilling a policy is simple to express in code.</span></span> <span data-ttu-id="9c9f8-163">É possível fornecer uma `Func<AuthorizationHandlerContext, bool>` ao configurar a política com o `RequireAssertion` criador de políticas.</span><span class="sxs-lookup"><span data-stu-id="9c9f8-163">It's possible to supply a `Func<AuthorizationHandlerContext, bool>` when configuring your policy with the `RequireAssertion` policy builder.</span></span>

<span data-ttu-id="9c9f8-164">Por exemplo, o anterior `BadgeEntryHandler` poderia ser reescrito da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="9c9f8-164">For example, the previous `BadgeEntryHandler` could be rewritten as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=52-53,57-63)]

## <a name="accessing-mvc-request-context-in-handlers"></a><span data-ttu-id="9c9f8-165">Acessando o contexto de solicitação do MVC em manipuladores</span><span class="sxs-lookup"><span data-stu-id="9c9f8-165">Accessing MVC request context in handlers</span></span>

<span data-ttu-id="9c9f8-166">O `HandleRequirementAsync` método implementar um manipulador de autorização tem dois parâmetros: um `AuthorizationHandlerContext` e o `TRequirement` manipulada.</span><span class="sxs-lookup"><span data-stu-id="9c9f8-166">The `HandleRequirementAsync` method you implement in an authorization handler has two parameters: an `AuthorizationHandlerContext` and the `TRequirement` you are handling.</span></span> <span data-ttu-id="9c9f8-167">Estruturas como MVC ou Jabbr serão livres para adicionar qualquer objeto para o `Resource` propriedade o `AuthorizationHandlerContext` para passar informações adicionais.</span><span class="sxs-lookup"><span data-stu-id="9c9f8-167">Frameworks such as MVC or Jabbr are free to add any object to the `Resource` property on the `AuthorizationHandlerContext` to pass extra information.</span></span>

<span data-ttu-id="9c9f8-168">Por exemplo, o MVC passa uma instância de [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) no `Resource` propriedade.</span><span class="sxs-lookup"><span data-stu-id="9c9f8-168">For example, MVC passes an instance of [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) in the `Resource` property.</span></span> <span data-ttu-id="9c9f8-169">Esta propriedade fornece acesso a `HttpContext`, `RouteData`e tudo pessoa forneceu MVC e páginas Razor.</span><span class="sxs-lookup"><span data-stu-id="9c9f8-169">This property provides access to `HttpContext`, `RouteData`, and everything else provided by MVC and Razor Pages.</span></span>

<span data-ttu-id="9c9f8-170">O uso do `Resource` é de propriedade específicos da estrutura.</span><span class="sxs-lookup"><span data-stu-id="9c9f8-170">The use of the `Resource` property is framework specific.</span></span> <span data-ttu-id="9c9f8-171">Usando informações de `Resource` propriedade limita suas políticas de autorização para estruturas específicas.</span><span class="sxs-lookup"><span data-stu-id="9c9f8-171">Using information in the `Resource` property limits your authorization policies to particular frameworks.</span></span> <span data-ttu-id="9c9f8-172">Você deve converter o `Resource` propriedade usando o `as` palavra-chave e, em seguida, confirme a conversão tenha êxito para garantir que seu código não falha com um `InvalidCastException` quando executado em outras estruturas:</span><span class="sxs-lookup"><span data-stu-id="9c9f8-172">You should cast the `Resource` property using the `as` keyword, and then confirm the cast has succeed to ensure your code doesn't crash with an `InvalidCastException` when run on other frameworks:</span></span>

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```
