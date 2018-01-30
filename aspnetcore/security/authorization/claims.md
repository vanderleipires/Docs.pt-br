---
title: "Autorização baseada em declarações"
author: rick-anderson
description: "Este documento explica como adicionar declarações verificações de autorização em um aplicativo do ASP.NET Core."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/claims
ms.openlocfilehash: 76b6566df4a427836eb5060f7d80e1039e479884
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2018
---
# <a name="claims-based-authorization"></a><span data-ttu-id="0438b-103">Autorização baseada em declarações</span><span class="sxs-lookup"><span data-stu-id="0438b-103">Claims-Based Authorization</span></span>

<a name="security-authorization-claims-based"></a>

<span data-ttu-id="0438b-104">Quando uma identidade é criada ele pode ser atribuído uma ou mais declarações emitidas por um terceiro confiável.</span><span class="sxs-lookup"><span data-stu-id="0438b-104">When an identity is created it may be assigned one or more claims issued by a trusted party.</span></span> <span data-ttu-id="0438b-105">Uma declaração é o valor de nome é um par que representa que o assunto, não que a entidade pode fazer.</span><span class="sxs-lookup"><span data-stu-id="0438b-105">A claim is name value pair that represents what the subject is, not what the subject can do.</span></span> <span data-ttu-id="0438b-106">Por exemplo, você pode ter de motorista uma carteira, emitida por uma autoridade de licença de um local.</span><span class="sxs-lookup"><span data-stu-id="0438b-106">For example, you may have a driver's license, issued by a local driving license authority.</span></span> <span data-ttu-id="0438b-107">Licença do driver tem sua data de nascimento.</span><span class="sxs-lookup"><span data-stu-id="0438b-107">Your driver's license has your date of birth on it.</span></span> <span data-ttu-id="0438b-108">Nesse caso seria o nome da declaração `DateOfBirth`, o valor da declaração seria sua data de nascimento, por exemplo `8th June 1970` e o emissor de um autoridade de licença.</span><span class="sxs-lookup"><span data-stu-id="0438b-108">In this case the claim name would be `DateOfBirth`, the claim value would be your date of birth, for example `8th June 1970` and the issuer would be the driving license authority.</span></span> <span data-ttu-id="0438b-109">Autorização baseada em declarações, em sua forma mais simples, verifica o valor de uma declaração e permite o acesso a um recurso com base no valor.</span><span class="sxs-lookup"><span data-stu-id="0438b-109">Claims based authorization, at its simplest, checks the value of a claim and allows access to a resource based upon that value.</span></span> <span data-ttu-id="0438b-110">Por exemplo, se você quiser que o processo de autorização de acesso para uma sociedade noite poderia ser:</span><span class="sxs-lookup"><span data-stu-id="0438b-110">For example if you want access to a night club the authorization process might be:</span></span>

<span data-ttu-id="0438b-111">A analista de segurança de porta deve avaliar o valor da sua data de nascimento declaração e se elas têm confiança do emissor (de um autoridade de licença) antes de conceder a que você acessar.</span><span class="sxs-lookup"><span data-stu-id="0438b-111">The door security officer would evaluate the value of your date of birth claim and whether they trust the issuer (the driving license authority) before granting you access.</span></span>

<span data-ttu-id="0438b-112">Uma identidade pode conter várias declarações com vários valores e pode conter várias declarações do mesmo tipo.</span><span class="sxs-lookup"><span data-stu-id="0438b-112">An identity can contain multiple claims with multiple values and can contain multiple claims of the same type.</span></span>

## <a name="adding-claims-checks"></a><span data-ttu-id="0438b-113">Adicionar declarações verificações</span><span class="sxs-lookup"><span data-stu-id="0438b-113">Adding claims checks</span></span>

<span data-ttu-id="0438b-114">Declaração de verificações de autorização com base são declarativas - o desenvolvedor incorpora dentro de seu código, em relação a um controlador ou uma ação dentro de um controlador, especificando que o usuário atual deve ter, e, opcionalmente, o valor de declaração deve conter para acessar o recurso solicitado.</span><span class="sxs-lookup"><span data-stu-id="0438b-114">Claim based authorization checks are declarative - the developer embeds them within their code, against a controller or an action within a controller, specifying claims which the current user must possess, and optionally the value the claim must hold to access the requested resource.</span></span> <span data-ttu-id="0438b-115">Declarações de requisitos são baseada em política, o desenvolvedor deve criar e registrar uma política de expressar os requisitos de declarações.</span><span class="sxs-lookup"><span data-stu-id="0438b-115">Claims requirements are policy based, the developer must build and register a policy expressing the claims requirements.</span></span>

<span data-ttu-id="0438b-116">O tipo mais simples de política procura a presença de uma declaração de declaração e não verifica o valor.</span><span class="sxs-lookup"><span data-stu-id="0438b-116">The simplest type of claim policy looks for the presence of a claim and doesn't check the value.</span></span>

<span data-ttu-id="0438b-117">Primeiro você precisa criar e registrar a política.</span><span class="sxs-lookup"><span data-stu-id="0438b-117">First you need to build and register the policy.</span></span> <span data-ttu-id="0438b-118">Isso ocorre como parte da configuração do serviço de autorização, que normalmente faz parte do `ConfigureServices()` no seu *Startup.cs* arquivo.</span><span class="sxs-lookup"><span data-stu-id="0438b-118">This takes place as part of the Authorization service configuration, which normally takes part in `ConfigureServices()` in your *Startup.cs* file.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("EmployeeOnly", policy => policy.RequireClaim("EmployeeNumber"));
    });
}
```

<span data-ttu-id="0438b-119">Nesse caso o `EmployeeOnly` política verifica a presença de um `EmployeeNumber` de declaração de identidade atual.</span><span class="sxs-lookup"><span data-stu-id="0438b-119">In this case the `EmployeeOnly` policy checks for the presence of an `EmployeeNumber` claim on the current identity.</span></span>

<span data-ttu-id="0438b-120">Em seguida, aplique a política usando o `Policy` propriedade no `AuthorizeAttribute` atributo para especificar o nome da política</span><span class="sxs-lookup"><span data-stu-id="0438b-120">You then apply the policy using the `Policy` property on the `AuthorizeAttribute` attribute to specify the policy name;</span></span>

```csharp
[Authorize(Policy = "EmployeeOnly")]
public IActionResult VacationBalance()
{
    return View();
}
```

<span data-ttu-id="0438b-121">O `AuthorizeAttribute` atributo pode ser aplicado a um controlador de inteiro, nesta instância, somente a política de correspondência de identidades terão acesso permitidas para qualquer ação no controlador.</span><span class="sxs-lookup"><span data-stu-id="0438b-121">The `AuthorizeAttribute` attribute can be applied to an entire controller, in this instance only identities matching the policy will be allowed access to any Action on the controller.</span></span>

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class VacationController : Controller
{
    public ActionResult VacationBalance()
    {
    }
}
```

<span data-ttu-id="0438b-122">Se você tiver um controlador que é protegido pelo `AuthorizeAttribute` de atributo, mas deseja permitir acesso anônimo a ações específicas que você aplicar o `AllowAnonymousAttribute` atributo.</span><span class="sxs-lookup"><span data-stu-id="0438b-122">If you have a controller that's protected by the `AuthorizeAttribute` attribute, but want to allow anonymous access to particular actions you apply the `AllowAnonymousAttribute` attribute.</span></span>

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class VacationController : Controller
{
    public ActionResult VacationBalance()
    {
    }

    [AllowAnonymous]
    public ActionResult VacationPolicy()
    {
    }
}
```

<span data-ttu-id="0438b-123">A maioria das declarações vem com um valor.</span><span class="sxs-lookup"><span data-stu-id="0438b-123">Most claims come with a value.</span></span> <span data-ttu-id="0438b-124">Você pode especificar uma lista de valores permitido ao criar a política.</span><span class="sxs-lookup"><span data-stu-id="0438b-124">You can specify a list of allowed values when creating the policy.</span></span> <span data-ttu-id="0438b-125">O exemplo a seguir seria êxito apenas para os funcionários cujo número de funcionário foi 1, 2, 3, 4 ou 5.</span><span class="sxs-lookup"><span data-stu-id="0438b-125">The following example would only succeed for employees whose employee number was 1, 2, 3, 4 or 5.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("Founders", policy =>
                          policy.RequireClaim("EmployeeNumber", "1", "2", "3", "4", "5"));
    });
}
```

## <a name="multiple-policy-evaluation"></a><span data-ttu-id="0438b-126">Vários avaliação de política</span><span class="sxs-lookup"><span data-stu-id="0438b-126">Multiple Policy Evaluation</span></span>

<span data-ttu-id="0438b-127">Se você aplicar várias políticas para um controlador ou ação, todas as políticas devem passar antes que o acesso é concedido.</span><span class="sxs-lookup"><span data-stu-id="0438b-127">If you apply multiple policies to a controller or action, then all policies must pass before access is granted.</span></span> <span data-ttu-id="0438b-128">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="0438b-128">For example:</span></span>

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class SalaryController : Controller
{
    public ActionResult Payslip()
    {
    }

    [Authorize(Policy = "HumanResources")]
    public ActionResult UpdateSalary()
    {
    }
}
```

<span data-ttu-id="0438b-129">No exemplo acima qualquer identidade que atende a `EmployeeOnly` política pode acessar o `Payslip` ação como essa política é aplicada no controlador.</span><span class="sxs-lookup"><span data-stu-id="0438b-129">In the above example any identity which fulfills the `EmployeeOnly` policy can access the `Payslip` action as that policy is enforced on the controller.</span></span> <span data-ttu-id="0438b-130">No entanto para chamar o `UpdateSalary` ação de identidade deve ser atendidos *ambos* o `EmployeeOnly` política e o `HumanResources` política.</span><span class="sxs-lookup"><span data-stu-id="0438b-130">However in order to call the `UpdateSalary` action the identity must fulfill *both* the `EmployeeOnly` policy and the `HumanResources` policy.</span></span>

<span data-ttu-id="0438b-131">Se você quiser políticas mais complicadas, como colocar uma data de nascimento declaração, calcular uma idade dele e verificando a idade for 21 ou anterior, você precisa gravar [manipuladores de política personalizada](policies.md).</span><span class="sxs-lookup"><span data-stu-id="0438b-131">If you want more complicated policies, such as taking a date of birth claim, calculating an age from it then checking the age is 21 or older then you need to write [custom policy handlers](policies.md).</span></span>
