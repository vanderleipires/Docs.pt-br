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
ms.openlocfilehash: 24585ed5b4c21a357fc0eed4de6ccedf9fa50d3e
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/13/2017
---
# <a name="custom-policy-based-authorization"></a>Autorização personalizada com base em políticas

<a name="security-authorization-policies-based"></a>

Nos bastidores a [autorização de função](roles.md) e [declarações de autorização](claims.md) fazer uso de um requisito, um manipulador para o requisito e uma política de pré-configurada. Esses blocos de construção que você express avaliações de autorização no código, permitindo uma avançada reutilizável e a estrutura de autorização facilmente testáveis.

Uma política de autorização é composta de um ou mais requisitos e registrada na inicialização do aplicativo como parte da configuração do serviço de autorização no `ConfigureServices` no *Startup.cs* arquivo.

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

Aqui você pode ver que uma política de "Over21" é criada com um requisito, que de idade mínima, que é passado como um parâmetro para o requisito.

As políticas são aplicadas usando o `Authorize` atributo especificando o nome da política, por exemplo,

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

## <a name="requirements"></a>Requisitos

Um requisito de autorização é uma coleção de parâmetros de dados que uma política pode usar para avaliar a entidade de segurança do usuário atual. Em nossa política de idade mínima o requisito que temos é um único parâmetro, a idade mínima. Um requisito deve implementar `IAuthorizationRequirement`. Esta é uma interface de marcador vazio. Um requisito de idade mínima com parâmetros pode ser implementado como se segue;

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

Um requisito não precisa ter dados ou propriedades.

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a>Manipuladores de autorização

Um manipulador de autorização é responsável pela avaliação de todas as propriedades de um requisito. O manipulador de autorização deve avaliar em relação a um fornecido `AuthorizationHandlerContext` para decidir se a autorização é permitida. Pode ter um requisito [vários manipuladores](policies.md#security-authorization-policies-based-multiple-handlers). Manipuladores devem herdar `AuthorizationHandler<T>` onde T é o requisito trata.

<a name="security-authorization-handler-example"></a>

O manipulador de idade mínima pode ter esta aparência:

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

No código acima é primeiro verificar se a entidade de usuário atual tem uma data de nascimento de declaração que foi emitido por um emissor sabemos e confiança. Se a declaração está ausente, não é possível autorizar para retornamos. Se houver uma declaração, podemos calcular quanto tempo o usuário existe e se eles atenderem a idade mínima passada pela necessidade, em seguida, autorização foi bem-sucedida. Após a autorização bem-sucedida, podemos chamar `context.Succeed()` passando o requisito de que tenha sido bem-sucedida como um parâmetro.

<a name="security-authorization-policies-based-handler-registration"></a>

Manipuladores devem ser registrados na coleção de serviços durante a configuração, por exemplo,

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

Cada manipulador é adicionado à coleção de serviços usando `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();` passando em sua classe de manipulador.

## <a name="what-should-a-handler-return"></a>O que deve retornar um manipulador?

Você pode ver no nosso [exemplo manipulador](policies.md#security-authorization-handler-example) que o `Handle()` método não tem nenhum valor de retorno, como podemos indicam sucesso ou falha?

* Um manipulador indica êxito chamando `context.Succeed(IAuthorizationRequirement requirement)`, passando o requisito de que foi validada com êxito.

* Um manipulador não precisa lidar com falhas em geral, como outros manipuladores para o mesmo requisito podem ser bem-sucedida.

* Para garantir a falha mesmo se um requisito de outros manipuladores de êxito, chame `context.Fail`.

Todos os manipuladores para um requisito, independentemente de você chamar dentro de seu manipulador serão chamados quando uma política requer que o requisito. Isso permite que os requisitos para ter efeitos colaterais, como o registro, que sempre ocorrerá mesmo se `context.Fail()` foi chamado no manipulador de outro.

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a>Por que eu quero vários manipuladores para um requisito?

Em casos onde você deseja evaluation para estar em um **ou** base implementar vários manipuladores para um requisito. Por exemplo, a Microsoft tem portas que apenas abrir com cartões de chave. Se você deixar o seu cartão de chave em casa ao recepcionista imprime uma etiqueta temporária e abre a porta para você. Nesse cenário, você teria um requisito, *EnterBuilding*, mas vários manipuladores, cada um deles examinando um requisito.

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

Agora, assumindo que ambos os manipuladores são [registrado](xref:security/authorization/policies#security-authorization-policies-based-handler-registration) quando uma política é avaliada a `EnterBuildingRequirement` se tiver êxito ou manipulador de avaliação da política terá êxito.

## <a name="using-a-func-to-fulfill-a-policy"></a>Usando uma função para atender a uma política

Pode haver ocasiões em que é simple de expressar em código que atendem a uma política. É possível simplesmente fornecer um `Func<AuthorizationHandlerContext, bool>` ao configurar a política com o `RequireAssertion` criador de políticas.

Por exemplo anterior `BadgeEntryHandler` poderia ser reescrito como se segue;

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

## <a name="accessing-mvc-request-context-in-handlers"></a>Acessando o contexto de solicitação do MVC em manipuladores

O `Handle` método você deve implementar um manipulador de autorização tem dois parâmetros, um `AuthorizationContext` e `Requirement` manipulada. Estruturas como MVC ou Jabbr serão livres para adicionar qualquer objeto para o `Resource` propriedade o `AuthorizationContext` para passar informações extras.

Por exemplo, MVC passa uma instância de `Microsoft.AspNetCore.Mvc.Filters.AuthorizationFilterContext` na propriedade de recurso que é usada para acessar o HttpContext, RouteData e tudo mais MVC fornece.

O uso do `Resource` é de propriedade específicos da estrutura. Usando informações de `Resource` propriedade limitará suas políticas de autorização para estruturas específicas. Você deve converter o `Resource` propriedade usando o `as` palavra-chave e, em seguida, verifique a conversão tenha êxito para garantir que seu código não falha com `InvalidCastExceptions` quando executado em outras estruturas;

```csharp
if (context.Resource is Microsoft.AspNetCore.Mvc.Filters.AuthorizationFilterContext mvcContext)
{
    // Examine MVC specific things like routing data.
}
```
