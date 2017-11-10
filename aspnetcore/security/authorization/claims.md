---
title: "Autorização baseada em declarações"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 737be5cd-3511-4f1c-b0ce-65403fb5eed3
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/claims
ms.openlocfilehash: 51d747c3c295164b6c0e00b5bafe63c565d588b4
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/13/2017
---
# <a name="claims-based-authorization"></a>Autorização baseada em declarações

<a name="security-authorization-claims-based"></a>

Quando uma identidade é criada ele pode ser atribuído uma ou mais declarações emitidas por um terceiro confiável. Uma declaração é o valor de nome é um par que representa que o assunto, não que a entidade pode fazer. Por exemplo, você pode ter uma licença de Drivers, emitido por uma autoridade de licença de um local. Licença do driver tem sua data de nascimento. Nesse caso seria o nome da declaração `DateOfBirth`, o valor da declaração seria sua data de nascimento, por exemplo `8th June 1970` e o emissor de um autoridade de licença. Autorização baseada em declarações, em sua forma mais simples, verifica o valor de uma declaração e permite o acesso a um recurso com base no valor. Por exemplo, se você quiser que o processo de autorização de acesso para uma sociedade noite poderia ser:

A analista de segurança de porta deve avaliar o valor da sua data de nascimento declaração e se elas têm confiança do emissor (de um autoridade de licença) antes de conceder a que você acessar.

Uma identidade pode conter várias declarações com vários valores e pode conter várias declarações do mesmo tipo.

## <a name="adding-claims-checks"></a>Adicionar declarações verificações

Declaração de verificações de autorização com base são declarativas - o desenvolvedor incorpora dentro de seu código, em relação a um controlador ou uma ação dentro de um controlador, especificando que o usuário atual deve ter, e, opcionalmente, o valor de declaração deve conter para acessar o recurso solicitado. Declarações de requisitos são baseada em política, o desenvolvedor deve criar e registrar uma política de expressar os requisitos de declarações.

O tipo mais simples de política procura a presença de uma declaração de declaração e não verifica o valor.

Primeiro você precisa criar e registrar a política. Isso ocorre como parte da configuração do serviço de autorização, que normalmente faz parte do `ConfigureServices()` no seu *Startup.cs* arquivo.

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

Nesse caso o `EmployeeOnly` política verifica a presença de um `EmployeeNumber` de declaração de identidade atual.

Em seguida, aplique a política usando o `Policy` propriedade no `AuthorizeAttribute` atributo para especificar o nome da política

```csharp
[Authorize(Policy = "EmployeeOnly")]
public IActionResult VacationBalance()
{
    return View();
}
```

O `AuthorizeAttribute` atributo pode ser aplicado a um controlador de inteiro, nesta instância, somente a política de correspondência de identidades terão acesso permitidas para qualquer ação no controlador.

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class VacationController : Controller
{
    public ActionResult VacationBalance()
    {
    }
}
```

Se você tiver um controlador que é protegido pelo `AuthorizeAttribute` de atributo, mas deseja permitir acesso anônimo a ações específicas que você aplicar o `AllowAnonymousAttribute` atributo.

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

A maioria das declarações vem com um valor. Você pode especificar uma lista de valores permitido ao criar a política. O exemplo a seguir seria êxito apenas para os funcionários cujo número de funcionário foi 1, 2, 3, 4 ou 5.

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

## <a name="multiple-policy-evaluation"></a>Vários avaliação de política

Se você aplicar várias políticas para um controlador ou ação todas as políticas devem passar antes que o acesso é concedido. Por exemplo:

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

No exemplo acima qualquer identidade que atende a `EmployeeOnly` política pode acessar o `Payslip` ação como essa política é aplicada no controlador. No entanto para chamar o `UpdateSalary` ação de identidade deve ser atendidos *ambos* o `EmployeeOnly` política e o `HumanResources` política.

Se você quiser políticas mais complicadas, como colocar uma data de nascimento declaração, calcular uma idade dele e verificando a idade for 21 ou anterior, você precisa gravar [manipuladores de política personalizada](policies.md#security-authorization-policies-based).
