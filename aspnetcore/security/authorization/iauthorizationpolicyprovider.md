---
title: Provedores de diretiva de autorização personalizada no núcleo do ASP.NET
author: mjrousos
description: Saiba como usar um IAuthorizationPolicyProvider personalizado em um aplicativo do ASP.NET Core para gerar dinamicamente as diretivas de autorização.
ms.author: riande
ms.custom: mvc
ms.date: 05/02/2018
uid: security/authorization/iauthorizationpolicyprovider
ms.openlocfilehash: 524928a5b291e02556d11a762d86430a6dc94660
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277251"
---
# <a name="custom-authorization-policy-providers-using-iauthorizationpolicyprovider-in-aspnet-core"></a><span data-ttu-id="84188-103">Provedores personalizados de política de autorização usando IAuthorizationPolicyProvider no núcleo do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="84188-103">Custom Authorization Policy Providers using IAuthorizationPolicyProvider in ASP.NET Core</span></span> 

<span data-ttu-id="84188-104">Por [Mike Rousos](https://github.com/mjrousos)</span><span class="sxs-lookup"><span data-stu-id="84188-104">By [Mike Rousos](https://github.com/mjrousos)</span></span>

<span data-ttu-id="84188-105">Normalmente ao usar [autorização baseada em política](xref:security/authorization/policies), as políticas são registradas chamando `AuthorizationOptions.AddPolicy` como parte da configuração do serviço de autorização.</span><span class="sxs-lookup"><span data-stu-id="84188-105">Typically when using [policy-based authorization](xref:security/authorization/policies), policies are registered by calling `AuthorizationOptions.AddPolicy` as part of authorization service configuration.</span></span> <span data-ttu-id="84188-106">Em alguns cenários, pode não ser possível (ou desejável) para registrar todas as políticas de autorização dessa maneira.</span><span class="sxs-lookup"><span data-stu-id="84188-106">In some scenarios, it may not be possible (or desirable) to register all authorization policies in this way.</span></span> <span data-ttu-id="84188-107">Nesses casos, você pode usar um personalizado `IAuthorizationPolicyProvider` para controlar como as políticas de autorização são fornecidas.</span><span class="sxs-lookup"><span data-stu-id="84188-107">In those cases, you can use a custom `IAuthorizationPolicyProvider` to control how authorization policies are supplied.</span></span>

<span data-ttu-id="84188-108">Exemplos de cenários onde um personalizado [IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) podem ser úteis incluem:</span><span class="sxs-lookup"><span data-stu-id="84188-108">Examples of scenarios where a custom [IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) may be useful include:</span></span>

* <span data-ttu-id="84188-109">Usando um serviço externo para fornecer a avaliação da política.</span><span class="sxs-lookup"><span data-stu-id="84188-109">Using an external service to provide policy evaluation.</span></span>
* <span data-ttu-id="84188-110">Usando uma grande variedade de políticas (para números de sala diferente ou anos, por exemplo), portanto não faz sentido para adicionar cada política de autorização individuais com um `AuthorizationOptions.AddPolicy` chamar.</span><span class="sxs-lookup"><span data-stu-id="84188-110">Using a large range of policies (for different room numbers or ages, for example), so it doesn’t make sense to add each individual authorization policy with an `AuthorizationOptions.AddPolicy` call.</span></span>
* <span data-ttu-id="84188-111">Criação de políticas em tempo de execução com base nas informações em uma fonte de dados externa (como um banco de dados) ou determinar os requisitos de autorização dinamicamente por meio de outro mecanismo.</span><span class="sxs-lookup"><span data-stu-id="84188-111">Creating policies at runtime based on information in an external data source (like a database) or determining authorization requirements dynamically through another mechanism.</span></span>

## <a name="customizing-policy-retrieval"></a><span data-ttu-id="84188-112">Personalizando a recuperação de política</span><span class="sxs-lookup"><span data-stu-id="84188-112">Customizing policy retrieval</span></span>

<span data-ttu-id="84188-113">Aplicativos do ASP.NET Core usam uma implementação do `IAuthorizationPolicyProvider` interface para recuperar as políticas de autorização.</span><span class="sxs-lookup"><span data-stu-id="84188-113">ASP.NET Core apps use an implementation of the `IAuthorizationPolicyProvider` interface to retrieve authorization policies.</span></span> <span data-ttu-id="84188-114">Por padrão, [DefaultAuthorizationPolicyProvider](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) é registrado e usado.</span><span class="sxs-lookup"><span data-stu-id="84188-114">By default, [DefaultAuthorizationPolicyProvider](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) is registered and used.</span></span> <span data-ttu-id="84188-115">`DefaultAuthorizationPolicyProvider` Retorna as políticas do `AuthorizationOptions` fornecido em um `IServiceCollection.AddAuthorization` chamar.</span><span class="sxs-lookup"><span data-stu-id="84188-115">`DefaultAuthorizationPolicyProvider` returns policies from the `AuthorizationOptions` provided in an `IServiceCollection.AddAuthorization` call.</span></span>

<span data-ttu-id="84188-116">Você pode personalizar esse comportamento ao registrar outro `IAuthorizationPolicyProvider` implementação no aplicativo do [injeção de dependência](xref:fundamentals/dependency-injection) contêiner.</span><span class="sxs-lookup"><span data-stu-id="84188-116">You can customize this behavior by registering a different `IAuthorizationPolicyProvider` implementation in the app’s [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> 

<span data-ttu-id="84188-117">O `IAuthorizationPolicyProvider` interface contém duas APIs:</span><span class="sxs-lookup"><span data-stu-id="84188-117">The `IAuthorizationPolicyProvider` interface contains two APIs:</span></span>

* <span data-ttu-id="84188-118">[GetPolicyAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) retorna uma política de autorização para um determinado nome.</span><span class="sxs-lookup"><span data-stu-id="84188-118">[GetPolicyAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) returns an authorization policy for a given name.</span></span>
* <span data-ttu-id="84188-119">[GetDefaultPolicyAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync?view=aspnetcore-2.0) retorna a política de autorização padrão (a política usada para `[Authorize]` atributos sem uma política especificada).</span><span class="sxs-lookup"><span data-stu-id="84188-119">[GetDefaultPolicyAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync?view=aspnetcore-2.0) returns the default authorization policy (the policy used for `[Authorize]` attributes without a policy specified).</span></span> 

<span data-ttu-id="84188-120">Ao implementar essas duas APIs, você pode personalizar como as políticas de autorização são fornecidas.</span><span class="sxs-lookup"><span data-stu-id="84188-120">By implementing these two APIs, you can customize how authorization policies are provided.</span></span>

## <a name="parameterized-authorize-attribute-example"></a><span data-ttu-id="84188-121">Parametrizado autorizar o exemplo de atributo</span><span class="sxs-lookup"><span data-stu-id="84188-121">Parameterized authorize attribute example</span></span>

<span data-ttu-id="84188-122">Um cenário onde `IAuthorizationPolicyProvider` é útil é habilitar personalizado `[Authorize]` atributos cujos requisitos dependem de um parâmetro.</span><span class="sxs-lookup"><span data-stu-id="84188-122">One scenario where `IAuthorizationPolicyProvider` is useful is enabling custom `[Authorize]` attributes whose requirements depend on a parameter.</span></span> <span data-ttu-id="84188-123">Por exemplo, em [autorização baseada em política](xref:security/authorization/policies) documentação, uma com base em idade ("AtLeast21") de política foi usada como um exemplo.</span><span class="sxs-lookup"><span data-stu-id="84188-123">For example, in [policy-based authorization](xref:security/authorization/policies) documentation, an age-based (“AtLeast21”) policy was used as a sample.</span></span> <span data-ttu-id="84188-124">Se as ações de controlador diferente em um aplicativo devem ser disponibilizadas para os usuários do *diferentes* anos, ele pode ser útil ter muitas políticas diferentes com base em idade.</span><span class="sxs-lookup"><span data-stu-id="84188-124">If different controller actions in an app should be made available to users of *different* ages, it might be useful to have many different age-based policies.</span></span> <span data-ttu-id="84188-125">Em vez de registrar todas as diferentes com base em idade políticas que o aplicativo será necessário em `AuthorizationOptions`, você pode gerar as políticas dinamicamente com um personalizado `IAuthorizationPolicyProvider`.</span><span class="sxs-lookup"><span data-stu-id="84188-125">Instead of registering all the different age-based policies that the application will need in `AuthorizationOptions`, you can generate the policies dynamically with a custom `IAuthorizationPolicyProvider`.</span></span> <span data-ttu-id="84188-126">Para certificar-se de usar as políticas mais fácil, você pode anotar as ações com o atributo de autorização personalizada como `[MinimumAgeAuthorize(20)]`.</span><span class="sxs-lookup"><span data-stu-id="84188-126">To make using the policies easier, you can annotate actions with custom authorization attribute like `[MinimumAgeAuthorize(20)]`.</span></span>

## <a name="custom-authorization-attributes"></a><span data-ttu-id="84188-127">Atributos de autorização personalizada</span><span class="sxs-lookup"><span data-stu-id="84188-127">Custom Authorization Attributes</span></span>

<span data-ttu-id="84188-128">Diretivas de autorização são identificadas por seus nomes.</span><span class="sxs-lookup"><span data-stu-id="84188-128">Authorization policies are identified by their names.</span></span> <span data-ttu-id="84188-129">Personalizado `MinimumAgeAuthorizeAttribute` descrito anteriormente precisa mapear argumentos em uma cadeia de caracteres que pode ser usada para recuperar a diretiva de autorização correspondente.</span><span class="sxs-lookup"><span data-stu-id="84188-129">The custom `MinimumAgeAuthorizeAttribute` described previously needs to map arguments into a string that can be used to retrieve the corresponding authorization policy.</span></span> <span data-ttu-id="84188-130">Você pode fazer isso derivando de `AuthorizeAttribute` e fazer a `Age` quebra automática de propriedade de `AuthorizeAttribute.Policy` propriedade.</span><span class="sxs-lookup"><span data-stu-id="84188-130">You can do this by deriving from `AuthorizeAttribute` and making the `Age` property wrap the `AuthorizeAttribute.Policy` property.</span></span>

```CSharp
internal class MinimumAgeAuthorizeAttribute : AuthorizeAttribute
{
    const string POLICY_PREFIX = "MinimumAge";

    public MinimumAgeAuthorizeAttribute(int age) => Age = age;

    // Get or set the Age property by manipulating the underlying Policy property
    public int Age
    {
        get
        {
            if (int.TryParse(Policy.Substring(POLICY_PREFIX.Length), out var age))
            {
                return age;
            }
            return default(int);
        }
        set
        {
            Policy = $"{POLICY_PREFIX}{value.ToString()}";
        }
    }
}
```

<span data-ttu-id="84188-131">Esse tipo de atributo tem um `Policy` cadeia de caracteres com base no prefixo embutido (`"MinimumAge"`) e um número inteiro passado por meio do construtor.</span><span class="sxs-lookup"><span data-stu-id="84188-131">This attribute type has a `Policy` string based on the hard-coded prefix (`"MinimumAge"`) and an integer passed in via the constructor.</span></span>

<span data-ttu-id="84188-132">Você pode aplicá-lo para ações da mesma maneira que outras `Authorize` atributos, exceto que assume um valor inteiro como um parâmetro.</span><span class="sxs-lookup"><span data-stu-id="84188-132">You can apply it to actions in the same way as other `Authorize` attributes except that it takes an integer as a parameter.</span></span>

```CSharp
[MinimumAgeAuthorize(10)]
public IActionResult RequiresMinimumAge10()
```

## <a name="custom-iauthorizationpolicyprovider"></a><span data-ttu-id="84188-133">IAuthorizationPolicyProvider personalizado</span><span class="sxs-lookup"><span data-stu-id="84188-133">Custom IAuthorizationPolicyProvider</span></span>

<span data-ttu-id="84188-134">Personalizado `MinimumAgeAuthorizeAttribute` torna mais fácil para políticas de autorização da solicitação para qualquer idade mínima desejada.</span><span class="sxs-lookup"><span data-stu-id="84188-134">The custom `MinimumAgeAuthorizeAttribute` makes it easy to request authorization policies for any minimum age desired.</span></span> <span data-ttu-id="84188-135">O próximo problema a resolver é verificar se as diretivas de autorização estão disponíveis para todos os anos diferentes.</span><span class="sxs-lookup"><span data-stu-id="84188-135">The next problem to solve is making sure that authorization policies are available for all of those different ages.</span></span> <span data-ttu-id="84188-136">Este é o local onde um `IAuthorizationPolicyProvider` é útil.</span><span class="sxs-lookup"><span data-stu-id="84188-136">This is where an `IAuthorizationPolicyProvider` is useful.</span></span>

<span data-ttu-id="84188-137">Ao usar `MinimumAgeAuthorizationAttribute`, os nomes de diretiva de autorização seguirá o padrão de `"MinimumAge" + Age`, de modo personalizado `IAuthorizationPolicyProvider` deve gerar diretivas de autorização por:</span><span class="sxs-lookup"><span data-stu-id="84188-137">When using `MinimumAgeAuthorizationAttribute`, the authorization policy names will follow the pattern `"MinimumAge" + Age`, so the custom `IAuthorizationPolicyProvider` should generate authorization policies by:</span></span>

* <span data-ttu-id="84188-138">A idade do nome da política de análise.</span><span class="sxs-lookup"><span data-stu-id="84188-138">Parsing the age from the policy name.</span></span>
* <span data-ttu-id="84188-139">Usando `AuthorizationPolicyBuiler` para criar um novo `AuthorizationPolicy`</span><span class="sxs-lookup"><span data-stu-id="84188-139">Using `AuthorizationPolicyBuiler` to create a new `AuthorizationPolicy`</span></span>
* <span data-ttu-id="84188-140">Adicionando requisitos para a política com base na idade com `AuthorizationPolicyBuilder.AddRequirements`.</span><span class="sxs-lookup"><span data-stu-id="84188-140">Adding requirements to the policy based on the age with `AuthorizationPolicyBuilder.AddRequirements`.</span></span> <span data-ttu-id="84188-141">Em outros cenários, você pode usar `RequireClaim`, `RequireRole`, ou `RequireUserName` em vez disso.</span><span class="sxs-lookup"><span data-stu-id="84188-141">In other scenarios, you might use `RequireClaim`, `RequireRole`, or `RequireUserName` instead.</span></span>

```CSharp
internal class MinimumAgePolicyProvider : IAuthorizationPolicyProvider
{
    const string POLICY_PREFIX = "MinimumAge";

    // Policies are looked up by string name, so expect 'parameters' (like age)
    // to be embedded in the policy names. This is abstracted away from developers
    // by the more strongly-typed attributes derived from AuthorizeAttribute
    // (like [MinimumAgeAuthorize()] in this sample)
    public Task<AuthorizationPolicy> GetPolicyAsync(string policyName)
    {
        if (policyName.StartsWith(POLICY_PREFIX, StringComparison.OrdinalIgnoreCase) &&
            int.TryParse(policyName.Substring(POLICY_PREFIX.Length), out var age))
        {
            var policy = new AuthorizationPolicyBuilder();
            policy.AddRequirements(new MinimumAgeRequirement(age));
            return Task.FromResult(policy.Build());
        }

        return Task.FromResult<AuthorizationPolicy>(null);
    }
}
```

## <a name="multiple-authorization-policy-providers"></a><span data-ttu-id="84188-142">Vários provedores de diretiva de autorização</span><span class="sxs-lookup"><span data-stu-id="84188-142">Multiple authorization policy providers</span></span>

<span data-ttu-id="84188-143">Quando usar custom `IAuthorizationPolicyProvider` implementações, tenha em mente que o ASP.NET Core usa apenas uma instância de `IAuthorizationPolicyProvider`.</span><span class="sxs-lookup"><span data-stu-id="84188-143">When using custom `IAuthorizationPolicyProvider` implementations, keep in mind that ASP.NET Core only uses one instance of `IAuthorizationPolicyProvider`.</span></span> <span data-ttu-id="84188-144">Se um provedor personalizado não é capaz de fornecer diretivas de autorização para todos os nomes de política, ele deve recorrer a um provedor de backup.</span><span class="sxs-lookup"><span data-stu-id="84188-144">If a custom provider isn't able to provide authorization policies for all policy names, it should fall back to a backup provider.</span></span> <span data-ttu-id="84188-145">Nomes de política podem incluir àqueles que vêm de uma política padrão para `[Authorize]` atributos sem nome.</span><span class="sxs-lookup"><span data-stu-id="84188-145">Policy names might include those that come from a default policy for `[Authorize]` attributes without a name.</span></span>

<span data-ttu-id="84188-146">Por exemplo, considere que um aplicativo precisar de políticas personalizadas de idade e recuperação de política com base em função mais tradicionais.</span><span class="sxs-lookup"><span data-stu-id="84188-146">For example, consider an application needed both custom age policies and more traditional role-based policy retrieval.</span></span> <span data-ttu-id="84188-147">Esse é um aplicativo pode usar um provedor de diretivas de autorização personalizada que:</span><span class="sxs-lookup"><span data-stu-id="84188-147">Such an app could use a custom authorization policy provider that:</span></span>

* <span data-ttu-id="84188-148">Tenta analisar nomes de política.</span><span class="sxs-lookup"><span data-stu-id="84188-148">Attempts to parse policy names.</span></span> 
* <span data-ttu-id="84188-149">Chamadas para um provedor de diretivas diferente (como `DefaultAuthorizationPolicyProvider`) se o nome da política não contiver uma idade.</span><span class="sxs-lookup"><span data-stu-id="84188-149">Calls into a different policy provider (like `DefaultAuthorizationPolicyProvider`) if the policy name doesn't contain an age.</span></span>

## <a name="default-policy"></a><span data-ttu-id="84188-150">Política padrão</span><span class="sxs-lookup"><span data-stu-id="84188-150">Default policy</span></span>

<span data-ttu-id="84188-151">Além de fornecer diretivas de autorização nomeado, um personalizado `IAuthorizationPolicyProvider` precisa implementar `GetDefaultPolicyAsync` para fornecer uma diretiva de autorização para `[Authorize]` atributos sem um nome de política especificada.</span><span class="sxs-lookup"><span data-stu-id="84188-151">In addition to providing named authorization policies, a custom `IAuthorizationPolicyProvider` needs to implement `GetDefaultPolicyAsync` to provide an authorization policy for `[Authorize]` attributes without a policy name specified.</span></span>

<span data-ttu-id="84188-152">Em muitos casos, esse atributo de autorização só requer um usuário autenticado, portanto você pode fazer a política necessário com uma chamada para `RequireAuthenticatedUser`:</span><span class="sxs-lookup"><span data-stu-id="84188-152">In many cases, this authorization attribute only requires an authenticated user, so you can make the necessary policy with a call to `RequireAuthenticatedUser`:</span></span>

```CSharp
public Task<AuthorizationPolicy> GetDefaultPolicyAsync() => 
    Task.FromResult(new AuthorizationPolicyBuilder().RequireAuthenticatedUser().Build());
```

<span data-ttu-id="84188-153">Assim como acontece com todos os aspectos de um personalizado `IAuthorizationPolicyProvider`, você pode personalizá-la, conforme necessário.</span><span class="sxs-lookup"><span data-stu-id="84188-153">As with all aspects of a custom `IAuthorizationPolicyProvider`, you can customize this, as needed.</span></span> <span data-ttu-id="84188-154">Em alguns casos:</span><span class="sxs-lookup"><span data-stu-id="84188-154">In some cases:</span></span>

* <span data-ttu-id="84188-155">Políticas de autorização padrão não podem ser usadas.</span><span class="sxs-lookup"><span data-stu-id="84188-155">Default authorization policies might not be used.</span></span>
* <span data-ttu-id="84188-156">Recuperando a diretiva padrão pode ser delegada a um fallback `IAuthorizationPolicyProvider`.</span><span class="sxs-lookup"><span data-stu-id="84188-156">Retrieving the default policy can be delegated to a fallback `IAuthorizationPolicyProvider`.</span></span>

## <a name="using-a-custom-iauthorizationpolicyprovider"></a><span data-ttu-id="84188-157">Usando um IAuthorizationPolicyProvider personalizado</span><span class="sxs-lookup"><span data-stu-id="84188-157">Using a Custom IAuthorizationPolicyProvider</span></span>

<span data-ttu-id="84188-158">Para usar políticas personalizadas de um `IAuthorizationPolicyProvider`, você deve:</span><span class="sxs-lookup"><span data-stu-id="84188-158">To use custom policies from an `IAuthorizationPolicyProvider`, you must:</span></span>

* <span data-ttu-id="84188-159">Registrar apropriada `AuthorizationHandler` tipos de injeção de dependência (descrito na [autorização baseada em política](xref:security/authorization/policies#authorization-handlers)), assim como acontece com todos os cenários de autorização baseada em política.</span><span class="sxs-lookup"><span data-stu-id="84188-159">Register the appropriate `AuthorizationHandler` types with dependency injection (described in [policy-based authorization](xref:security/authorization/policies#authorization-handlers)), as with all policy-based authorization scenarios.</span></span>
* <span data-ttu-id="84188-160">Registrar personalizado `IAuthorizationPolicyProvider` tipo na coleção de serviço de injeção de dependência do aplicativo (em `Startup.ConfigureServices`) para substituir o provedor de política padrão.</span><span class="sxs-lookup"><span data-stu-id="84188-160">Register the custom `IAuthorizationPolicyProvider` type in the app's dependency injection service collection (in `Startup.ConfigureServices`) to replace the default policy provider.</span></span>

```CSharp
services.AddTransient<IAuthorizationPolicyProvider, MinimumAgePolicyProvider>();
```

<span data-ttu-id="84188-161">Um personalizado completo `IAuthorizationPolicyProvider` está disponível no exemplo o [repositório do GitHub aspnet/AuthSamples](https://github.com/aspnet/AuthSamples/tree/dev/samples/CustomPolicyProvider).</span><span class="sxs-lookup"><span data-stu-id="84188-161">A complete custom `IAuthorizationPolicyProvider` sample is available in the [aspnet/AuthSamples GitHub repository](https://github.com/aspnet/AuthSamples/tree/dev/samples/CustomPolicyProvider).</span></span>
