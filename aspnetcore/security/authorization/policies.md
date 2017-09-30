---
title: "Autorização personalizada com base em políticas"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: e422a1b2-dc4a-4bcc-b8d9-7ee62009b6a3
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/policies
ms.openlocfilehash: 5021b5d20f6d9b9a4d8889f25b5e41f2c9306f64
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/28/2017
---
# <a name="custom-policy-based-authorization"></a><span data-ttu-id="8797d-103">Autorização personalizada com base em políticas</span><span class="sxs-lookup"><span data-stu-id="8797d-103">Custom Policy-Based Authorization</span></span>

<a name=security-authorization-policies-based></a>

<span data-ttu-id="8797d-104">Nos bastidores a [autorização de função](roles.md#security-authorization-role-based) e [declarações de autorização](claims.md#security-authorization-claims-based) fazer uso de um requisito, um manipulador para o requisito e uma política de pré-configurada.</span><span class="sxs-lookup"><span data-stu-id="8797d-104">Underneath the covers the [role authorization](roles.md#security-authorization-role-based) and [claims authorization](claims.md#security-authorization-claims-based) make use of a requirement, a handler for the requirement and a pre-configured policy.</span></span> <span data-ttu-id="8797d-105">Esses blocos de construção que você express avaliações de autorização no código, permitindo uma avançada reutilizável e a estrutura de autorização facilmente testáveis.</span><span class="sxs-lookup"><span data-stu-id="8797d-105">These building blocks allow you to express authorization evaluations in code, allowing for a richer, reusable, and easily testable authorization structure.</span></span>

<span data-ttu-id="8797d-106">Uma política de autorização é composta de um ou mais requisitos e registrada na inicialização do aplicativo como parte da configuração do serviço de autorização no `ConfigureServices` no *Startup.cs* arquivo.</span><span class="sxs-lookup"><span data-stu-id="8797d-106">An authorization policy is made up of one or more requirements and registered at application startup as part of the Authorization service configuration, in `ConfigureServices` in the *Startup.cs* file.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("Over21",
                          policy => policy.Requirements.Add(new MinimumAgeRequirement(21)));
    });
}
```

<span data-ttu-id="8797d-107">Aqui você pode ver que uma política de "Over21" é criada com um requisito, que de idade mínima, que é passado como um parâmetro para o requisito.</span><span class="sxs-lookup"><span data-stu-id="8797d-107">Here you can see an "Over21" policy is created with a single requirement, that of a minimum age, which is passed as a parameter to the requirement.</span></span>

<span data-ttu-id="8797d-108">As políticas são aplicadas usando o `Authorize` atributo especificando o nome da política, por exemplo,</span><span class="sxs-lookup"><span data-stu-id="8797d-108">Policies are applied using the `Authorize` attribute by specifying the policy name, for example;</span></span>

```csharp
[Authorize(Policy="Over21")]
public class AlcoholPurchaseRequirementsController : Controller
{
    public ActionResult Login()
    {
    }

    public ActionResult Logout()
    {
    }
}
```

## <a name="requirements"></a><span data-ttu-id="8797d-109">Requisitos</span><span class="sxs-lookup"><span data-stu-id="8797d-109">Requirements</span></span>

<span data-ttu-id="8797d-110">Um requisito de autorização é uma coleção de parâmetros de dados que uma política pode usar para avaliar a entidade de segurança do usuário atual.</span><span class="sxs-lookup"><span data-stu-id="8797d-110">An authorization requirement is a collection of data parameters that a policy can use to evaluate the current user principal.</span></span> <span data-ttu-id="8797d-111">Em nossa política de idade mínima o requisito que temos é um único parâmetro, a idade mínima.</span><span class="sxs-lookup"><span data-stu-id="8797d-111">In our Minimum Age policy the requirement we have is a single parameter, the minimum age.</span></span> <span data-ttu-id="8797d-112">Um requisito deve implementar `IAuthorizationRequirement`.</span><span class="sxs-lookup"><span data-stu-id="8797d-112">A requirement must implement `IAuthorizationRequirement`.</span></span> <span data-ttu-id="8797d-113">Esta é uma interface de marcador vazio.</span><span class="sxs-lookup"><span data-stu-id="8797d-113">This is an empty, marker interface.</span></span> <span data-ttu-id="8797d-114">Um requisito de idade mínima com parâmetros pode ser implementado como se segue;</span><span class="sxs-lookup"><span data-stu-id="8797d-114">A parameterized minimum age requirement might be implemented as follows;</span></span>

```csharp
public class MinimumAgeRequirement : IAuthorizationRequirement
{
    public int MinimumAge { get; private set; }
    
    public MinimumAgeRequirement(int minimumAge)
    {
        MinimumAge = minimumAge;
    }
}
```

<span data-ttu-id="8797d-115">Um requisito não precisa ter dados ou propriedades.</span><span class="sxs-lookup"><span data-stu-id="8797d-115">A requirement doesn't need to have data or properties.</span></span>

<a name=security-authorization-policies-based-authorization-handler></a>

## <a name="authorization-handlers"></a><span data-ttu-id="8797d-116">Manipuladores de autorização</span><span class="sxs-lookup"><span data-stu-id="8797d-116">Authorization Handlers</span></span>

<span data-ttu-id="8797d-117">Um manipulador de autorização é responsável pela avaliação de todas as propriedades de um requisito.</span><span class="sxs-lookup"><span data-stu-id="8797d-117">An authorization handler is responsible for the evaluation of any properties of a requirement.</span></span> <span data-ttu-id="8797d-118">O manipulador de autorização deve avaliar em relação a um fornecido `AuthorizationHandlerContext` para decidir se a autorização é permitida.</span><span class="sxs-lookup"><span data-stu-id="8797d-118">The  authorization handler must evaluate them against a provided `AuthorizationHandlerContext` to decide if authorization is allowed.</span></span> <span data-ttu-id="8797d-119">Pode ter um requisito [vários manipuladores](policies.md#security-authorization-policies-based-multiple-handlers).</span><span class="sxs-lookup"><span data-stu-id="8797d-119">A requirement can have [multiple handlers](policies.md#security-authorization-policies-based-multiple-handlers).</span></span> <span data-ttu-id="8797d-120">Manipuladores devem herdar `AuthorizationHandler<T>` onde T é o requisito trata.</span><span class="sxs-lookup"><span data-stu-id="8797d-120">Handlers must inherit `AuthorizationHandler<T>` where T is the requirement it handles.</span></span>

<a name=security-authorization-handler-example></a>

<span data-ttu-id="8797d-121">O manipulador de idade mínima pode ter esta aparência:</span><span class="sxs-lookup"><span data-stu-id="8797d-121">The minimum age handler might look like this:</span></span>

```csharp
public class MinimumAgeHandler : AuthorizationHandler<MinimumAgeRequirement>
{
    protected override Task HandleRequirementAsync(AuthorizationHandlerContext context, MinimumAgeRequirement requirement)
    {
        if (!context.User.HasClaim(c => c.Type == ClaimTypes.DateOfBirth &&
                                   c.Issuer == "http://contoso.com"))
        {
            // .NET 4.x -> return Task.FromResult(0);
            return Task.CompletedTask;
        }

        var dateOfBirth = Convert.ToDateTime(context.User.FindFirst(
            c => c.Type == ClaimTypes.DateOfBirth && c.Issuer == "http://contoso.com").Value);

        int calculatedAge = DateTime.Today.Year - dateOfBirth.Year;
        if (dateOfBirth > DateTime.Today.AddYears(-calculatedAge))
        {
            calculatedAge--;
        }

        if (calculatedAge >= requirement.MinimumAge)
        {
            context.Succeed(requirement);
        }
        return Task.CompletedTask;
    }
}
```

<span data-ttu-id="8797d-122">No código acima é primeiro verificar se a entidade de usuário atual tem uma data de nascimento de declaração que foi emitido por um emissor sabemos e confiança.</span><span class="sxs-lookup"><span data-stu-id="8797d-122">In the code above we first look to see if the current user principal has a date of birth claim which has been issued by an Issuer we know and trust.</span></span> <span data-ttu-id="8797d-123">Se a declaração está ausente, não é possível autorizar para retornamos.</span><span class="sxs-lookup"><span data-stu-id="8797d-123">If the claim is missing we can't authorize so we return.</span></span> <span data-ttu-id="8797d-124">Se houver uma declaração, podemos calcular quanto tempo o usuário existe e se eles atenderem a idade mínima passada pela necessidade, em seguida, autorização foi bem-sucedida.</span><span class="sxs-lookup"><span data-stu-id="8797d-124">If we have a claim, we figure out how old the user is, and if they meet the minimum age passed in by the requirement then authorization has been successful.</span></span> <span data-ttu-id="8797d-125">Após a autorização bem-sucedida, podemos chamar `context.Succeed()` passando o requisito de que tenha sido bem-sucedida como um parâmetro.</span><span class="sxs-lookup"><span data-stu-id="8797d-125">Once authorization is successful we call `context.Succeed()` passing in the requirement that has been successful as a parameter.</span></span>

<a name=security-authorization-policies-based-handler-registration></a>

<span data-ttu-id="8797d-126">Manipuladores devem ser registrados na coleção de serviços durante a configuração, por exemplo,</span><span class="sxs-lookup"><span data-stu-id="8797d-126">Handlers must be registered in the services collection during configuration, for example;</span></span>

```csharp

public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("Over21",
                          policy => policy.Requirements.Add(new MinimumAgeRequirement(21)));
    });

    services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();
}
```

<span data-ttu-id="8797d-127">Cada manipulador é adicionado à coleção de serviços usando `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();` passando em sua classe de manipulador.</span><span class="sxs-lookup"><span data-stu-id="8797d-127">Each handler is added to the services collection by using `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();` passing in your handler class.</span></span>

## <a name="what-should-a-handler-return"></a><span data-ttu-id="8797d-128">O que deve retornar um manipulador?</span><span class="sxs-lookup"><span data-stu-id="8797d-128">What should a handler return?</span></span>

<span data-ttu-id="8797d-129">Você pode ver no nosso [exemplo manipulador](policies.md#security-authorization-handler-example) que o `Handle()` método não tem nenhum valor de retorno, como podemos indicam sucesso ou falha?</span><span class="sxs-lookup"><span data-stu-id="8797d-129">You can see in our [handler example](policies.md#security-authorization-handler-example) that the `Handle()` method has no return value, so how do we indicate success or failure?</span></span>

* <span data-ttu-id="8797d-130">Um manipulador indica êxito chamando `context.Succeed(IAuthorizationRequirement requirement)`, passando o requisito de que foi validada com êxito.</span><span class="sxs-lookup"><span data-stu-id="8797d-130">A handler indicates success by calling `context.Succeed(IAuthorizationRequirement requirement)`, passing the requirement that has been successfully validated.</span></span>

* <span data-ttu-id="8797d-131">Um manipulador não precisa lidar com falhas em geral, como outros manipuladores para o mesmo requisito podem ser bem-sucedida.</span><span class="sxs-lookup"><span data-stu-id="8797d-131">A handler does not need to handle failures generally, as other handlers for the same requirement may succeed.</span></span>

* <span data-ttu-id="8797d-132">Para garantir a falha mesmo se um requisito de outros manipuladores de êxito, chame `context.Fail`.</span><span class="sxs-lookup"><span data-stu-id="8797d-132">To guarantee failure even if other handlers for a requirement succeed, call `context.Fail`.</span></span>

<span data-ttu-id="8797d-133">Todos os manipuladores para um requisito, independentemente de você chamar dentro de seu manipulador serão chamados quando uma política requer que o requisito.</span><span class="sxs-lookup"><span data-stu-id="8797d-133">Regardless of what you call inside your handler all handlers for a requirement will be called when a policy requires the requirement.</span></span> <span data-ttu-id="8797d-134">Isso permite que os requisitos para ter efeitos colaterais, como o registro, que sempre ocorrerá mesmo se `context.Fail()` foi chamado no manipulador de outro.</span><span class="sxs-lookup"><span data-stu-id="8797d-134">This allows requirements to have side effects, such as logging, which will always take place even if `context.Fail()` has been called in another handler.</span></span>

<a name=security-authorization-policies-based-multiple-handlers></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a><span data-ttu-id="8797d-135">Por que eu quero vários manipuladores para um requisito?</span><span class="sxs-lookup"><span data-stu-id="8797d-135">Why would I want multiple handlers for a requirement?</span></span>

<span data-ttu-id="8797d-136">Em casos onde você deseja evaluation para estar em um **ou** base implementar vários manipuladores para um requisito.</span><span class="sxs-lookup"><span data-stu-id="8797d-136">In cases where you want evaluation to be on an **OR** basis you implement multiple handlers for a single requirement.</span></span> <span data-ttu-id="8797d-137">Por exemplo, a Microsoft tem portas que apenas abrir com cartões de chave.</span><span class="sxs-lookup"><span data-stu-id="8797d-137">For example, Microsoft has doors which only open with key cards.</span></span> <span data-ttu-id="8797d-138">Se você deixar o seu cartão de chave em casa ao recepcionista imprime uma etiqueta temporária e abre a porta para você.</span><span class="sxs-lookup"><span data-stu-id="8797d-138">If you leave your key card at home the receptionist prints a temporary sticker and opens the door for you.</span></span> <span data-ttu-id="8797d-139">Nesse cenário, você teria um requisito, *EnterBuilding*, mas vários manipuladores, cada um deles examinando um requisito.</span><span class="sxs-lookup"><span data-stu-id="8797d-139">In this scenario you'd have a single requirement, *EnterBuilding*, but multiple handlers, each one examining a single requirement.</span></span>

```csharp
public class EnterBuildingRequirement : IAuthorizationRequirement
{
}

public class BadgeEntryHandler : AuthorizationHandler<EnterBuildingRequirement>
{
    protected override Task HandleRequirementAsync(AuthorizationHandlerContext context, EnterBuildingRequirement requirement)
    {
        if (context.User.HasClaim(c => c.Type == ClaimTypes.BadgeId &&
                                       c.Issuer == "http://microsoftsecurity"))
        {
            context.Succeed(requirement);
        }
        return Task.CompletedTask;
    }
}

public class HasTemporaryStickerHandler : AuthorizationHandler<EnterBuildingRequirement>
{
    protected override Task HandleRequirementAsync(AuthorizationHandlerContext context, EnterBuildingRequirement requirement)
    {
        if (context.User.HasClaim(c => c.Type == ClaimTypes.TemporaryBadgeId &&
                                       c.Issuer == "https://microsoftsecurity"))
        {
            // We'd also check the expiration date on the sticker.
            context.Succeed(requirement);
        }
        return Task.CompletedTask;
    }
}
```

<span data-ttu-id="8797d-140">Agora, assumindo que ambos os manipuladores são [registrado](xref:security/authorization/policies#security-authorization-policies-based-handler-registration) quando uma política é avaliada a `EnterBuildingRequirement` se tiver êxito ou manipulador de avaliação da política terá êxito.</span><span class="sxs-lookup"><span data-stu-id="8797d-140">Now, assuming both handlers are [registered](xref:security/authorization/policies#security-authorization-policies-based-handler-registration) when a policy evaluates the `EnterBuildingRequirement` if either handler succeeds the policy evaluation will succeed.</span></span>

## <a name="using-a-func-to-fulfill-a-policy"></a><span data-ttu-id="8797d-141">Usando uma função para atender a uma política</span><span class="sxs-lookup"><span data-stu-id="8797d-141">Using a func to fulfill a policy</span></span>

<span data-ttu-id="8797d-142">Pode haver ocasiões em que é simple de expressar em código que atendem a uma política.</span><span class="sxs-lookup"><span data-stu-id="8797d-142">There may be occasions where fulfilling a policy is simple to express in code.</span></span> <span data-ttu-id="8797d-143">É possível simplesmente fornecer um `Func<AuthorizationHandlerContext, bool>` ao configurar a política com o `RequireAssertion` criador de políticas.</span><span class="sxs-lookup"><span data-stu-id="8797d-143">It is possible to simply supply a `Func<AuthorizationHandlerContext, bool>` when configuring your policy with the `RequireAssertion` policy builder.</span></span>

<span data-ttu-id="8797d-144">Por exemplo anterior `BadgeEntryHandler` poderia ser reescrito como se segue;</span><span class="sxs-lookup"><span data-stu-id="8797d-144">For example the previous `BadgeEntryHandler` could be rewritten as follows;</span></span>

```csharp
services.AddAuthorization(options =>
    {
        options.AddPolicy("BadgeEntry",
                          policy => policy.RequireAssertion(context =>
                                  context.User.HasClaim(c =>
                                     (c.Type == ClaimTypes.BadgeId ||
                                      c.Type == ClaimTypes.TemporaryBadgeId)
                                      && c.Issuer == "https://microsoftsecurity"));
                          }));
    }
 }
```

## <a name="accessing-mvc-request-context-in-handlers"></a><span data-ttu-id="8797d-145">Acessando o contexto de solicitação do MVC em manipuladores</span><span class="sxs-lookup"><span data-stu-id="8797d-145">Accessing MVC Request Context In Handlers</span></span>

<span data-ttu-id="8797d-146">O `Handle` método você deve implementar um manipulador de autorização tem dois parâmetros, um `AuthorizationContext` e `Requirement` manipulada.</span><span class="sxs-lookup"><span data-stu-id="8797d-146">The `Handle` method you must implement in an authorization handler has two parameters, an `AuthorizationContext` and the `Requirement` you are handling.</span></span> <span data-ttu-id="8797d-147">Estruturas como MVC ou Jabbr serão livres para adicionar qualquer objeto para o `Resource` propriedade o `AuthorizationContext` para passar informações extras.</span><span class="sxs-lookup"><span data-stu-id="8797d-147">Frameworks such as MVC or Jabbr are free to add any object to the `Resource` property on the `AuthorizationContext` to pass through extra information.</span></span>

<span data-ttu-id="8797d-148">Por exemplo, MVC passa uma instância de `Microsoft.AspNetCore.Mvc.Filters.AuthorizationFilterContext` na propriedade de recurso que é usada para acessar o HttpContext, RouteData e tudo mais MVC fornece.</span><span class="sxs-lookup"><span data-stu-id="8797d-148">For example MVC passes an instance of `Microsoft.AspNetCore.Mvc.Filters.AuthorizationFilterContext` in the resource property which is used to access HttpContext, RouteData and everything else MVC provides.</span></span>

<span data-ttu-id="8797d-149">O uso do `Resource` é de propriedade específicos da estrutura.</span><span class="sxs-lookup"><span data-stu-id="8797d-149">The use of the `Resource` property is framework specific.</span></span> <span data-ttu-id="8797d-150">Usando informações de `Resource` propriedade limitará suas políticas de autorização para estruturas específicas.</span><span class="sxs-lookup"><span data-stu-id="8797d-150">Using information in the `Resource` property will limit your authorization policies to particular frameworks.</span></span> <span data-ttu-id="8797d-151">Você deve converter o `Resource` propriedade usando o `as` palavra-chave e, em seguida, verifique a conversão tenha êxito para garantir que seu código não falha com `InvalidCastExceptions` quando executado em outras estruturas;</span><span class="sxs-lookup"><span data-stu-id="8797d-151">You should cast the `Resource` property using the `as` keyword, and then check the cast has succeed to ensure your code doesn't crash with `InvalidCastExceptions` when run on other frameworks;</span></span>

```csharp
if (context.Resource is Microsoft.AspNetCore.Mvc.Filters.AuthorizationFilterContext mvcContext)
{
    // Examine MVC specific things like routing data.
}
```
