---
title: "Autorização personalizada com base em políticas"
author: rick-anderson
description: "Este documento explica como criar e usar manipuladores de diretiva de autorização personalizada em um aplicativo do ASP.NET Core."
keywords: "ASP.NET Core, autorização, a política personalizada, a política de autorização"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: e422a1b2-dc4a-4bcc-b8d9-7ee62009b6a3
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/policies
ms.openlocfilehash: 0281d054204a11acc2cf11cf5fca23a8f70aad8e
ms.sourcegitcommit: 037d3900f739dbaa2ba14158e3d7dc81478952ad
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/01/2017
---
# <a name="custom-policy-based-authorization"></a><span data-ttu-id="77fa3-104">Autorização personalizada com base em políticas</span><span class="sxs-lookup"><span data-stu-id="77fa3-104">Custom policy-based authorization</span></span>

<a name="security-authorization-policies-based"></a>

<span data-ttu-id="77fa3-105">Nos bastidores, o [autorização de função](roles.md) e [declarações de autorização](claims.md) fazer uso de um requisito, um manipulador para o requisito e uma política de pré-configurada.</span><span class="sxs-lookup"><span data-stu-id="77fa3-105">Underneath the covers, the [role authorization](roles.md) and [claims authorization](claims.md) make use of a requirement, a handler for the requirement, and a pre-configured policy.</span></span> <span data-ttu-id="77fa3-106">Esses blocos de construção que você express avaliações de autorização no código, permitindo uma avançada reutilizável e a estrutura de autorização facilmente testáveis.</span><span class="sxs-lookup"><span data-stu-id="77fa3-106">These building blocks allow you to express authorization evaluations in code, allowing for a richer, reusable, and easily testable authorization structure.</span></span>

<span data-ttu-id="77fa3-107">Uma política de autorização é composta de um ou mais requisitos e registrada na inicialização do aplicativo como parte da configuração do serviço de autorização no `ConfigureServices` no *Startup.cs* arquivo.</span><span class="sxs-lookup"><span data-stu-id="77fa3-107">An authorization policy is made up of one or more requirements and registered at application startup as part of the Authorization service configuration, in `ConfigureServices` in the *Startup.cs* file.</span></span>

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

<span data-ttu-id="77fa3-108">Aqui você pode ver que uma política de "Over21" é criada com um requisito, que de idade mínima, que é passado como um parâmetro para o requisito.</span><span class="sxs-lookup"><span data-stu-id="77fa3-108">Here you can see an "Over21" policy is created with a single requirement, that of a minimum age, which is passed as a parameter to the requirement.</span></span>

<span data-ttu-id="77fa3-109">As políticas são aplicadas usando o `Authorize` atributo especificando o nome da política, por exemplo,</span><span class="sxs-lookup"><span data-stu-id="77fa3-109">Policies are applied using the `Authorize` attribute by specifying the policy name, for example;</span></span>

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

## <a name="requirements"></a><span data-ttu-id="77fa3-110">Requisitos</span><span class="sxs-lookup"><span data-stu-id="77fa3-110">Requirements</span></span>

<span data-ttu-id="77fa3-111">Um requisito de autorização é uma coleção de parâmetros de dados que uma política pode usar para avaliar a entidade de segurança do usuário atual.</span><span class="sxs-lookup"><span data-stu-id="77fa3-111">An authorization requirement is a collection of data parameters that a policy can use to evaluate the current user principal.</span></span> <span data-ttu-id="77fa3-112">Em nossa política de idade mínima, o requisito que temos é um único parâmetro, a idade mínima.</span><span class="sxs-lookup"><span data-stu-id="77fa3-112">In our Minimum Age policy, the requirement we have is a single parameter, the minimum age.</span></span> <span data-ttu-id="77fa3-113">Um requisito deve implementar `IAuthorizationRequirement`.</span><span class="sxs-lookup"><span data-stu-id="77fa3-113">A requirement must implement `IAuthorizationRequirement`.</span></span> <span data-ttu-id="77fa3-114">Esta é uma interface de marcador vazio.</span><span class="sxs-lookup"><span data-stu-id="77fa3-114">This is an empty, marker interface.</span></span> <span data-ttu-id="77fa3-115">Um requisito de idade mínima com parâmetros pode ser implementado como se segue;</span><span class="sxs-lookup"><span data-stu-id="77fa3-115">A parameterized minimum age requirement might be implemented as follows;</span></span>

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

<span data-ttu-id="77fa3-116">Um requisito não precisa ter dados ou propriedades.</span><span class="sxs-lookup"><span data-stu-id="77fa3-116">A requirement doesn't need to have data or properties.</span></span>

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a><span data-ttu-id="77fa3-117">Manipuladores de autorização</span><span class="sxs-lookup"><span data-stu-id="77fa3-117">Authorization handlers</span></span>

<span data-ttu-id="77fa3-118">Um manipulador de autorização é responsável pela avaliação de todas as propriedades de um requisito.</span><span class="sxs-lookup"><span data-stu-id="77fa3-118">An authorization handler is responsible for the evaluation of any properties of a requirement.</span></span> <span data-ttu-id="77fa3-119">O manipulador de autorização deve avaliar em relação a um fornecido `AuthorizationHandlerContext` para decidir se a autorização é permitida.</span><span class="sxs-lookup"><span data-stu-id="77fa3-119">The  authorization handler must evaluate them against a provided `AuthorizationHandlerContext` to decide if authorization is allowed.</span></span> <span data-ttu-id="77fa3-120">Pode ter um requisito [vários manipuladores](policies.md#security-authorization-policies-based-multiple-handlers).</span><span class="sxs-lookup"><span data-stu-id="77fa3-120">A requirement can have [multiple handlers](policies.md#security-authorization-policies-based-multiple-handlers).</span></span> <span data-ttu-id="77fa3-121">Manipuladores devem herdar `AuthorizationHandler<T>` onde T é o requisito trata.</span><span class="sxs-lookup"><span data-stu-id="77fa3-121">Handlers must inherit `AuthorizationHandler<T>` where T is the requirement it handles.</span></span>

<a name="security-authorization-handler-example"></a>

<span data-ttu-id="77fa3-122">O manipulador de idade mínima pode ter esta aparência:</span><span class="sxs-lookup"><span data-stu-id="77fa3-122">The minimum age handler might look like this:</span></span>

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

<span data-ttu-id="77fa3-123">No código acima, é primeiro verificar se a entidade de usuário atual tem uma data de nascimento de declaração que foi emitido por um emissor sabemos e confiança.</span><span class="sxs-lookup"><span data-stu-id="77fa3-123">In the code above, we first look to see if the current user principal has a date of birth claim which has been issued by an Issuer we know and trust.</span></span> <span data-ttu-id="77fa3-124">Se a declaração está ausente, não é possível autorizar para retornamos.</span><span class="sxs-lookup"><span data-stu-id="77fa3-124">If the claim is missing we can't authorize so we return.</span></span> <span data-ttu-id="77fa3-125">Se houver uma declaração, podemos calcular quanto tempo o usuário existe e se eles atenderem a idade mínima passada pela necessidade, em seguida, autorização foi bem-sucedida.</span><span class="sxs-lookup"><span data-stu-id="77fa3-125">If we have a claim, we figure out how old the user is, and if they meet the minimum age passed in by the requirement then authorization has been successful.</span></span> <span data-ttu-id="77fa3-126">Após a autorização bem-sucedida, podemos chamar `context.Succeed()` passando o requisito de que tenha sido bem-sucedida como um parâmetro.</span><span class="sxs-lookup"><span data-stu-id="77fa3-126">Once authorization is successful we call `context.Succeed()` passing in the requirement that has been successful as a parameter.</span></span>

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a><span data-ttu-id="77fa3-127">Registro do manipulador</span><span class="sxs-lookup"><span data-stu-id="77fa3-127">Handler registration</span></span>
<span data-ttu-id="77fa3-128">Manipuladores devem ser registrados na coleção de serviços durante a configuração, por exemplo,</span><span class="sxs-lookup"><span data-stu-id="77fa3-128">Handlers must be registered in the services collection during configuration, for example;</span></span>

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

<span data-ttu-id="77fa3-129">Cada manipulador é adicionado à coleção de serviços usando `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();` passando em sua classe de manipulador.</span><span class="sxs-lookup"><span data-stu-id="77fa3-129">Each handler is added to the services collection by using `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();` passing in your handler class.</span></span>

## <a name="what-should-a-handler-return"></a><span data-ttu-id="77fa3-130">O que deve retornar um manipulador?</span><span class="sxs-lookup"><span data-stu-id="77fa3-130">What should a handler return?</span></span>

<span data-ttu-id="77fa3-131">Você pode ver no nosso [exemplo manipulador](policies.md#security-authorization-handler-example) que o `Handle()` método não tem nenhum valor de retorno, como podemos indicam sucesso ou falha?</span><span class="sxs-lookup"><span data-stu-id="77fa3-131">You can see in our [handler example](policies.md#security-authorization-handler-example) that the `Handle()` method has no return value, so how do we indicate success or failure?</span></span>

* <span data-ttu-id="77fa3-132">Um manipulador indica êxito chamando `context.Succeed(IAuthorizationRequirement requirement)`, passando o requisito de que foi validada com êxito.</span><span class="sxs-lookup"><span data-stu-id="77fa3-132">A handler indicates success by calling `context.Succeed(IAuthorizationRequirement requirement)`, passing the requirement that has been successfully validated.</span></span>

* <span data-ttu-id="77fa3-133">Um manipulador não precisa lidar com falhas em geral, como outros manipuladores para o mesmo requisito podem ser bem-sucedida.</span><span class="sxs-lookup"><span data-stu-id="77fa3-133">A handler does not need to handle failures generally, as other handlers for the same requirement may succeed.</span></span>

* <span data-ttu-id="77fa3-134">Para garantir a falha mesmo se um requisito de outros manipuladores de êxito, chame `context.Fail`.</span><span class="sxs-lookup"><span data-stu-id="77fa3-134">To guarantee failure even if other handlers for a requirement succeed, call `context.Fail`.</span></span>

<span data-ttu-id="77fa3-135">Todos os manipuladores para um requisito, independentemente do que você chamar dentro de seu manipulador, serão chamados quando uma política requer que o requisito.</span><span class="sxs-lookup"><span data-stu-id="77fa3-135">Regardless of what you call inside your handler, all handlers for a requirement will be called when a policy requires the requirement.</span></span> <span data-ttu-id="77fa3-136">Isso permite que os requisitos para ter efeitos colaterais, como o registro, que sempre ocorrerá mesmo se `context.Fail()` foi chamado no manipulador de outro.</span><span class="sxs-lookup"><span data-stu-id="77fa3-136">This allows requirements to have side effects, such as logging, which will always take place even if `context.Fail()` has been called in another handler.</span></span>

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a><span data-ttu-id="77fa3-137">Por que eu quero vários manipuladores para um requisito?</span><span class="sxs-lookup"><span data-stu-id="77fa3-137">Why would I want multiple handlers for a requirement?</span></span>

<span data-ttu-id="77fa3-138">Em casos onde você deseja evaluation para estar em um **ou** base implementar vários manipuladores para um requisito.</span><span class="sxs-lookup"><span data-stu-id="77fa3-138">In cases where you want evaluation to be on an **OR** basis you implement multiple handlers for a single requirement.</span></span> <span data-ttu-id="77fa3-139">Por exemplo, a Microsoft tem portas que apenas abrir com cartões de chave.</span><span class="sxs-lookup"><span data-stu-id="77fa3-139">For example, Microsoft has doors which only open with key cards.</span></span> <span data-ttu-id="77fa3-140">Se você deixar o seu cartão de chave em casa ao recepcionista imprime uma etiqueta temporária e abre a porta para você.</span><span class="sxs-lookup"><span data-stu-id="77fa3-140">If you leave your key card at home the receptionist prints a temporary sticker and opens the door for you.</span></span> <span data-ttu-id="77fa3-141">Nesse cenário, você teria um requisito, *EnterBuilding*, mas vários manipuladores, cada um deles examinando um requisito.</span><span class="sxs-lookup"><span data-stu-id="77fa3-141">In this scenario you'd have a single requirement, *EnterBuilding*, but multiple handlers, each one examining a single requirement.</span></span>

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

<span data-ttu-id="77fa3-142">Agora, assumindo que ambos os manipuladores são [registrado](xref:security/authorization/policies#security-authorization-policies-based-handler-registration) quando uma política é avaliada a `EnterBuildingRequirement` se tiver êxito ou manipulador de avaliação da política terá êxito.</span><span class="sxs-lookup"><span data-stu-id="77fa3-142">Now, assuming both handlers are [registered](xref:security/authorization/policies#security-authorization-policies-based-handler-registration) when a policy evaluates the `EnterBuildingRequirement` if either handler succeeds the policy evaluation will succeed.</span></span>

## <a name="using-a-func-to-fulfill-a-policy"></a><span data-ttu-id="77fa3-143">Usando uma função para atender a uma política</span><span class="sxs-lookup"><span data-stu-id="77fa3-143">Using a func to fulfill a policy</span></span>

<span data-ttu-id="77fa3-144">Pode haver ocasiões em que é simple de expressar em código que atendem a uma política.</span><span class="sxs-lookup"><span data-stu-id="77fa3-144">There may be occasions where fulfilling a policy is simple to express in code.</span></span> <span data-ttu-id="77fa3-145">É possível simplesmente fornecer um `Func<AuthorizationHandlerContext, bool>` ao configurar a política com o `RequireAssertion` criador de políticas.</span><span class="sxs-lookup"><span data-stu-id="77fa3-145">It is possible to simply supply a `Func<AuthorizationHandlerContext, bool>` when configuring your policy with the `RequireAssertion` policy builder.</span></span>

<span data-ttu-id="77fa3-146">Por exemplo anterior `BadgeEntryHandler` poderia ser reescrito da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="77fa3-146">For example the previous `BadgeEntryHandler` could be rewritten as follows:</span></span>

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

## <a name="accessing-mvc-request-context-in-handlers"></a><span data-ttu-id="77fa3-147">Acessando o contexto de solicitação do MVC em manipuladores</span><span class="sxs-lookup"><span data-stu-id="77fa3-147">Accessing MVC request context in handlers</span></span>

<span data-ttu-id="77fa3-148">O `Handle` método você deve implementar um manipulador de autorização tem dois parâmetros, um `AuthorizationContext` e `Requirement` manipulada.</span><span class="sxs-lookup"><span data-stu-id="77fa3-148">The `Handle` method you must implement in an authorization handler has two parameters, an `AuthorizationContext` and the `Requirement` you are handling.</span></span> <span data-ttu-id="77fa3-149">Estruturas como MVC ou Jabbr serão livres para adicionar qualquer objeto para o `Resource` propriedade o `AuthorizationContext` para passar informações extras.</span><span class="sxs-lookup"><span data-stu-id="77fa3-149">Frameworks such as MVC or Jabbr are free to add any object to the `Resource` property on the `AuthorizationContext` to pass through extra information.</span></span>

<span data-ttu-id="77fa3-150">Por exemplo, o MVC passa uma instância de `Microsoft.AspNetCore.Mvc.Filters.AuthorizationFilterContext` na propriedade de recurso que é usada para acessar o HttpContext, RouteData e tudo mais MVC fornece.</span><span class="sxs-lookup"><span data-stu-id="77fa3-150">For example, MVC passes an instance of `Microsoft.AspNetCore.Mvc.Filters.AuthorizationFilterContext` in the resource property which is used to access HttpContext, RouteData and everything else MVC provides.</span></span>

<span data-ttu-id="77fa3-151">O uso do `Resource` é de propriedade específicos da estrutura.</span><span class="sxs-lookup"><span data-stu-id="77fa3-151">The use of the `Resource` property is framework specific.</span></span> <span data-ttu-id="77fa3-152">Usando informações de `Resource` propriedade limitará suas políticas de autorização para estruturas específicas.</span><span class="sxs-lookup"><span data-stu-id="77fa3-152">Using information in the `Resource` property will limit your authorization policies to particular frameworks.</span></span> <span data-ttu-id="77fa3-153">Você deve converter o `Resource` propriedade usando o `as` palavra-chave e, em seguida, verifique a conversão tenha êxito para garantir que seu código não falha com `InvalidCastExceptions` quando executado em outras estruturas;</span><span class="sxs-lookup"><span data-stu-id="77fa3-153">You should cast the `Resource` property using the `as` keyword, and then check the cast has succeed to ensure your code doesn't crash with `InvalidCastExceptions` when run on other frameworks;</span></span>

```csharp
if (context.Resource is Microsoft.AspNetCore.Mvc.Filters.AuthorizationFilterContext mvcContext)
{
    // Examine MVC specific things like routing data.
}
```
