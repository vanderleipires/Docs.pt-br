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
# <a name="custom-authorization-policy-providers-using-iauthorizationpolicyprovider-in-aspnet-core"></a>Provedores personalizados de política de autorização usando IAuthorizationPolicyProvider no núcleo do ASP.NET 

Por [Mike Rousos](https://github.com/mjrousos)

Normalmente ao usar [autorização baseada em política](xref:security/authorization/policies), as políticas são registradas chamando `AuthorizationOptions.AddPolicy` como parte da configuração do serviço de autorização. Em alguns cenários, pode não ser possível (ou desejável) para registrar todas as políticas de autorização dessa maneira. Nesses casos, você pode usar um personalizado `IAuthorizationPolicyProvider` para controlar como as políticas de autorização são fornecidas.

Exemplos de cenários onde um personalizado [IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) podem ser úteis incluem:

* Usando um serviço externo para fornecer a avaliação da política.
* Usando uma grande variedade de políticas (para números de sala diferente ou anos, por exemplo), portanto não faz sentido para adicionar cada política de autorização individuais com um `AuthorizationOptions.AddPolicy` chamar.
* Criação de políticas em tempo de execução com base nas informações em uma fonte de dados externa (como um banco de dados) ou determinar os requisitos de autorização dinamicamente por meio de outro mecanismo.

## <a name="customizing-policy-retrieval"></a>Personalizando a recuperação de política

Aplicativos do ASP.NET Core usam uma implementação do `IAuthorizationPolicyProvider` interface para recuperar as políticas de autorização. Por padrão, [DefaultAuthorizationPolicyProvider](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) é registrado e usado. `DefaultAuthorizationPolicyProvider` Retorna as políticas do `AuthorizationOptions` fornecido em um `IServiceCollection.AddAuthorization` chamar.

Você pode personalizar esse comportamento ao registrar outro `IAuthorizationPolicyProvider` implementação no aplicativo do [injeção de dependência](xref:fundamentals/dependency-injection) contêiner. 

O `IAuthorizationPolicyProvider` interface contém duas APIs:

* [GetPolicyAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) retorna uma política de autorização para um determinado nome.
* [GetDefaultPolicyAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync?view=aspnetcore-2.0) retorna a política de autorização padrão (a política usada para `[Authorize]` atributos sem uma política especificada). 

Ao implementar essas duas APIs, você pode personalizar como as políticas de autorização são fornecidas.

## <a name="parameterized-authorize-attribute-example"></a>Parametrizado autorizar o exemplo de atributo

Um cenário onde `IAuthorizationPolicyProvider` é útil é habilitar personalizado `[Authorize]` atributos cujos requisitos dependem de um parâmetro. Por exemplo, em [autorização baseada em política](xref:security/authorization/policies) documentação, uma com base em idade ("AtLeast21") de política foi usada como um exemplo. Se as ações de controlador diferente em um aplicativo devem ser disponibilizadas para os usuários do *diferentes* anos, ele pode ser útil ter muitas políticas diferentes com base em idade. Em vez de registrar todas as diferentes com base em idade políticas que o aplicativo será necessário em `AuthorizationOptions`, você pode gerar as políticas dinamicamente com um personalizado `IAuthorizationPolicyProvider`. Para certificar-se de usar as políticas mais fácil, você pode anotar as ações com o atributo de autorização personalizada como `[MinimumAgeAuthorize(20)]`.

## <a name="custom-authorization-attributes"></a>Atributos de autorização personalizada

Diretivas de autorização são identificadas por seus nomes. Personalizado `MinimumAgeAuthorizeAttribute` descrito anteriormente precisa mapear argumentos em uma cadeia de caracteres que pode ser usada para recuperar a diretiva de autorização correspondente. Você pode fazer isso derivando de `AuthorizeAttribute` e fazer a `Age` quebra automática de propriedade de `AuthorizeAttribute.Policy` propriedade.

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

Esse tipo de atributo tem um `Policy` cadeia de caracteres com base no prefixo embutido (`"MinimumAge"`) e um número inteiro passado por meio do construtor.

Você pode aplicá-lo para ações da mesma maneira que outras `Authorize` atributos, exceto que assume um valor inteiro como um parâmetro.

```CSharp
[MinimumAgeAuthorize(10)]
public IActionResult RequiresMinimumAge10()
```

## <a name="custom-iauthorizationpolicyprovider"></a>IAuthorizationPolicyProvider personalizado

Personalizado `MinimumAgeAuthorizeAttribute` torna mais fácil para políticas de autorização da solicitação para qualquer idade mínima desejada. O próximo problema a resolver é verificar se as diretivas de autorização estão disponíveis para todos os anos diferentes. Este é o local onde um `IAuthorizationPolicyProvider` é útil.

Ao usar `MinimumAgeAuthorizationAttribute`, os nomes de diretiva de autorização seguirá o padrão de `"MinimumAge" + Age`, de modo personalizado `IAuthorizationPolicyProvider` deve gerar diretivas de autorização por:

* A idade do nome da política de análise.
* Usando `AuthorizationPolicyBuiler` para criar um novo `AuthorizationPolicy`
* Adicionando requisitos para a política com base na idade com `AuthorizationPolicyBuilder.AddRequirements`. Em outros cenários, você pode usar `RequireClaim`, `RequireRole`, ou `RequireUserName` em vez disso.

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

## <a name="multiple-authorization-policy-providers"></a>Vários provedores de diretiva de autorização

Quando usar custom `IAuthorizationPolicyProvider` implementações, tenha em mente que o ASP.NET Core usa apenas uma instância de `IAuthorizationPolicyProvider`. Se um provedor personalizado não é capaz de fornecer diretivas de autorização para todos os nomes de política, ele deve recorrer a um provedor de backup. Nomes de política podem incluir àqueles que vêm de uma política padrão para `[Authorize]` atributos sem nome.

Por exemplo, considere que um aplicativo precisar de políticas personalizadas de idade e recuperação de política com base em função mais tradicionais. Esse é um aplicativo pode usar um provedor de diretivas de autorização personalizada que:

* Tenta analisar nomes de política. 
* Chamadas para um provedor de diretivas diferente (como `DefaultAuthorizationPolicyProvider`) se o nome da política não contiver uma idade.

## <a name="default-policy"></a>Política padrão

Além de fornecer diretivas de autorização nomeado, um personalizado `IAuthorizationPolicyProvider` precisa implementar `GetDefaultPolicyAsync` para fornecer uma diretiva de autorização para `[Authorize]` atributos sem um nome de política especificada.

Em muitos casos, esse atributo de autorização só requer um usuário autenticado, portanto você pode fazer a política necessário com uma chamada para `RequireAuthenticatedUser`:

```CSharp
public Task<AuthorizationPolicy> GetDefaultPolicyAsync() => 
    Task.FromResult(new AuthorizationPolicyBuilder().RequireAuthenticatedUser().Build());
```

Assim como acontece com todos os aspectos de um personalizado `IAuthorizationPolicyProvider`, você pode personalizá-la, conforme necessário. Em alguns casos:

* Políticas de autorização padrão não podem ser usadas.
* Recuperando a diretiva padrão pode ser delegada a um fallback `IAuthorizationPolicyProvider`.

## <a name="using-a-custom-iauthorizationpolicyprovider"></a>Usando um IAuthorizationPolicyProvider personalizado

Para usar políticas personalizadas de um `IAuthorizationPolicyProvider`, você deve:

* Registrar apropriada `AuthorizationHandler` tipos de injeção de dependência (descrito na [autorização baseada em política](xref:security/authorization/policies#authorization-handlers)), assim como acontece com todos os cenários de autorização baseada em política.
* Registrar personalizado `IAuthorizationPolicyProvider` tipo na coleção de serviço de injeção de dependência do aplicativo (em `Startup.ConfigureServices`) para substituir o provedor de política padrão.

```CSharp
services.AddTransient<IAuthorizationPolicyProvider, MinimumAgePolicyProvider>();
```

Um personalizado completo `IAuthorizationPolicyProvider` está disponível no exemplo o [repositório do GitHub aspnet/AuthSamples](https://github.com/aspnet/AuthSamples/tree/dev/samples/CustomPolicyProvider).
