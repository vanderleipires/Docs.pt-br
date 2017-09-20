---
title: "Usando a autenticação de Cookie sem identidade do ASP.NET Core"
author: rick-anderson
description: "Obter uma explicação de como usar a autenticação de cookie sem a identidade do ASP.NET Core"
keywords: ASP.NET Core, cookies
ms.author: riande
manager: wpickett
ms.date: 08/14/2017
ms.topic: article
ms.assetid: 2bdcbf95-8d9d-4537-a4a0-a5ee439dcb62
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/cookie
ms.openlocfilehash: b728c3d62b59f28f1d020b6f3732918a1fcdf4eb
ms.sourcegitcommit: 74a8ad9c1ba5c155d7c4303e67632a0922c38e86
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/20/2017
---
# <a name="using-cookie-authentication-without-aspnet-core-identity"></a>Usando a autenticação de Cookie sem identidade do ASP.NET Core

<a name="security-authentication-cookie-middleware"></a>

ASP.NET Core 1. x fornece o cookie [middleware](../../fundamentals/middleware.md#fundamentals-middleware) que serializa um objeto de usuário em um cookie criptografado e, em seguida, em solicitações subsequentes, valida o cookie recria a entidade de segurança e a atribui para a `HttpContext.User` propriedade . Se você quiser fornecer suas próprias telas de logon e bancos de dados de usuário, você pode usar o middleware de cookie como um recurso autônomo.

Uma alteração importante no núcleo do ASP.NET 2. x é que o middleware de cookie ausente. Em vez disso, o `UseAuthentication` invocação de método no `Configure` método *Startup.cs* adiciona o AuthenticationMiddleware que define o `HttpContext.User` propriedade.

<a name="security-authentication-cookie-middleware-configuring"></a>

## <a name="adding-and-configuring"></a>Adicionando e configurando

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Conclua as seguintes etapas:

- Se não usar o `Microsoft.AspNetCore.All` [metapackage](xref:fundamentals/metapackage), instale a versão 2.0 + do `Microsoft.AspNetCore.Authentication.Cookies` pacote NuGet em seu projeto.

- Invocar o `UseAuthentication` método no `Configure` método o *Startup.cs* arquivo:

    ```csharp
    app.UseAuthentication();
    ```

- Invocar o `AddAuthentication` e `AddCookie` métodos no `ConfigureServices` método o *Startup.cs* arquivo:

    ```csharp
    services.AddAuthentication("MyCookieAuthenticationScheme")
            .AddCookie(options => {
                options.AccessDeniedPath = "/Account/Forbidden/";
                options.LoginPath = "/Account/Unauthorized/";
            });
    ```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Conclua as seguintes etapas:

- Instalar o `Microsoft.AspNetCore.Authentication.Cookies` pacote NuGet em seu projeto. Este pacote contém o middleware de cookie.

- Adicione as seguintes linhas para o `Configure` método no seu *Startup.cs* arquivo antes do `app.UseMvc()` instrução:

    ```csharp
    app.UseCookieAuthentication(new CookieAuthenticationOptions()
    {
        AccessDeniedPath = "/Account/Forbidden/",
        AuthenticationScheme = "MyCookieAuthenticationScheme",
        AutomaticAuthenticate = true,
        AutomaticChallenge = true,
        LoginPath = "/Account/Unauthorized/"
    });
    ```

---

Os trechos de código acima configurar algumas ou todas as seguintes opções:

* `AccessDeniedPath`-Este é o caminho relativo para o qual solicitações redirecionam quando um usuário tenta acessar um recurso, mas não passar por nenhuma [diretivas de autorização](xref:security/authorization/policies#security-authorization-policies-based) para esse recurso.

* `AuthenticationScheme`-Este é um valor pelo qual um esquema de autenticação de cookie específico é conhecido. Isso é útil quando há várias instâncias de autenticação de cookie e você deseja [limitar autorização para uma instância](xref:security/authorization/limitingidentitybyscheme#security-authorization-limiting-by-scheme).

* `AutomaticAuthenticate`-Este sinalizador é relevante apenas para o ASP.NET Core 1. x. Isso indica que a autenticação de cookie deve executar em cada solicitação e tente validar e reconstrua qualquer entidade de segurança serializada criado por ele.

* `AutomaticChallenge`-Este sinalizador é relevante apenas para o ASP.NET Core 1. x. Indica que a autenticação de cookie 1. x deve redirecionar o navegador para o `LoginPath` ou `AccessDeniedPath` quando ocorre falha na autorização.

* `LoginPath`-Este é o caminho relativo para o qual solicitações redirecionam quando um usuário tenta acessar um recurso, mas não foi autenticado.

[Outras opções](xref:security/authentication/cookie#security-authentication-cookie-options) incluem a capacidade de definir o emissor para quaisquer declarações que cria a autenticação de cookie, o nome do cookie de autenticação for removido, o domínio para o cookie e várias propriedades de segurança no cookie. Por padrão, a autenticação de cookie usa opções de segurança apropriadas para os cookies que cria, tais como:
- A definição do sinalizador HttpOnly para impedir o acesso de cookie no JavaScript do lado do cliente
- Limitando o cookie para HTTPS, se uma solicitação se mover sobre HTTPS

<a name="security-authentication-cookie-middleware-creating-a-cookie"></a>

## <a name="creating-an-identity-cookie"></a>Criando um cookie de identidade

Para criar um cookie que contém as informações do usuário, você precisa construir uma [ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal) contendo as informações que você deseja ser serializado no cookie. Uma vez que um adequado `ClaimsPrincipal` de objeto, chame o seguinte dentro de seu método de controlador:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync("MyCookieAuthenticationScheme", principal);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync("MyCookieAuthenticationScheme", principal);
```

---

Isso cria um cookie criptografado e o adiciona à resposta atual. O `AuthenticationScheme` especificado durante a [configuração](xref:security/authentication/cookie#security-authentication-cookie-middleware-configuring) deve ser usado ao chamar `SignInAsync`.

Nos bastidores, a criptografia é ASP.NET Core [proteção de dados](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) sistema. Se você estiver hospedando em vários computadores, carga balanceamento ou usando uma web farm, em seguida, você precisa [configurar a proteção de dados](xref:security/data-protection/configuration/overview#data-protection-configuring) para usar o mesmo anel de chave e o identificador do aplicativo.

## <a name="signing-out"></a>Sair

Para desconectar o usuário atual e exclua seus cookies, chame a seguir em seu controlador:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
await HttpContext.SignOutAsync("MyCookieAuthenticationScheme");
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignOutAsync("MyCookieAuthenticationScheme");
```

---

## <a name="reacting-to-back-end-changes"></a>Reagir a alterações de back-end

>[!WARNING]
> Quando um cookie principal tiver sido criado, ele se torna a única fonte de identidade. Mesmo se você desabilitar um usuário nos seus sistemas de back-end, a autenticação de cookie não tem conhecimento sobre isso e um usuário permanece conectado como o cookie é válido.

A autenticação de cookie fornece uma série de eventos em sua classe de opção. O `ValidateAsync()` evento pode ser usado para interceptar e ignorar a validação da identidade do cookie.

Considere um banco de dados de usuário de back-end que pode ter uma coluna de "LastChanged". Para invalidar um cookie quando o banco de dados é alterado, você deve primeiro, quando [criar o cookie](xref:security/authentication/cookie#security-authentication-cookie-middleware-creating-a-cookie), adicione uma declaração de "LastChanged" que contém o valor atual. Quando o banco de dados for alterado, o valor de "LastChanged" deve ser atualizado.

Para implementar uma substituição para o `ValidateAsync()` evento, você deve escrever um método com a seguinte assinatura:

```csharp
Task ValidateAsync(CookieValidatePrincipalContext context);
```

A identidade do ASP.NET Core implementa essa verificação como parte de seu `SecurityStampValidator`. Um exemplo é semelhante ao seguinte:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
public static class LastChangedValidator
{
    public static async Task ValidateAsync(CookieValidatePrincipalContext context)
    {
        // Pull database from registered DI services.
        var userRepository = context.HttpContext.RequestServices.GetRequiredService<IUserRepository>();
        var userPrincipal = context.Principal;

        // Look for the last changed claim.
        string lastChanged;
        lastChanged = (from c in userPrincipal.Claims
                        where c.Type == "LastUpdated"
                        select c.Value).FirstOrDefault();

        if (string.IsNullOrEmpty(lastChanged) ||
            !userRepository.ValidateLastChanged(userPrincipal, lastChanged))
        {
            context.RejectPrincipal();
            await context.HttpContext.SignOutAsync("MyCookieAuthenticationScheme");
        }
    }
}
```

Isso seria, em seguida, ser ligado durante o registro do serviço de cookie no `ConfigureServices` método *Startup.cs*:

```csharp
services.AddAuthentication("MyCookieAuthenticationScheme")
        .AddCookie(options =>
        {
            options.Events = new CookieAuthenticationEvents
            {
                OnValidatePrincipal = LastChangedValidator.ValidateAsync
            };
        });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
public static class LastChangedValidator
{
    public static async Task ValidateAsync(CookieValidatePrincipalContext context)
    {
        // Pull database from registered DI services.
        var userRepository = context.HttpContext.RequestServices.GetRequiredService<IUserRepository>();
        var userPrincipal = context.Principal;

        // Look for the last changed claim.
        string lastChanged;
        lastChanged = (from c in userPrincipal.Claims
                        where c.Type == "LastUpdated"
                        select c.Value).FirstOrDefault();

        if (string.IsNullOrEmpty(lastChanged) ||
            !userRepository.ValidateLastChanged(userPrincipal, lastChanged))
        {
            context.RejectPrincipal();
            await context.HttpContext.Authentication.SignOutAsync("MyCookieAuthenticationScheme");
        }
    }
}
```

Isso seria, em seguida, ser ligado durante a configuração de autenticação de cookie no `Configure` método *Startup.cs*:

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    Events = new CookieAuthenticationEvents
    {
        OnValidatePrincipal = LastChangedValidator.ValidateAsync
    }
});
```

---

Considere o exemplo no qual seu nome foi atualizado &mdash; uma decisão que não afeta a segurança de qualquer forma. Se você quiser atualizar não destrutivo a entidade de usuário, você pode chamar `context.ReplacePrincipal()` e defina o `context.ShouldRenew` propriedade `true`.

<a name="security-authentication-cookie-options"></a>

## <a name="controlling-cookie-options"></a>Controlando opções de cookie

O [CookieAuthenticationOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) classe vem com várias opções de configuração para ajustar os cookies que está sendo criados.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

2. x unifica as APIs usadas para configurar os cookies do ASP.NET Core. 1. x que APIs foram marcados como obsoletos e um novo `Cookie` propriedade do tipo `CookieBuilder` foi introduzido no `CookieAuthenticationOptions` classe. É recomendável que você migre para APIs 2. x.

* `ClaimsIssuer`o emissor a ser usado para o [emissor](https://docs.microsoft.com/dotnet/api/system.security.claims.claim.issuer) propriedade em quaisquer declarações criados pela autenticação de cookie.

* `CookieBuilder.Domain`é o nome de domínio ao qual o cookie é atendido. Por padrão, essa é a solicitação foi enviada para o nome do host. O navegador serve apenas o cookie para um nome de host correspondente. Talvez você queira ajustar isso para que os cookies disponíveis para qualquer host no seu domínio. Por exemplo, definir o domínio do cookie `.contoso.com` disponibiliza para `contoso.com`, `www.contoso.com`, `staging.www.contoso.com`, etc.

* `CookieBuilder.HttpOnly`é um sinalizador que indica se o cookie deve ser acessível somente para servidores. O padrão é `true`. A alteração desse valor pode abrir o aplicativo roubo de cookies de seu aplicativo precisar de um bug de scripts entre sites.

* `CookieBuilder.Path`pode ser usado para isolar aplicativos em execução no mesmo nome de host. Se você tiver um aplicativo em execução no `/app1` e para limitar os cookies emitidos para só será enviado para esse aplicativo, você deve definir o `CookiePath` propriedade `/app1`. Fazendo isso, o cookie só está disponível para solicitações para `/app1` ou algo abaixo dele.

* `CookieBuilder.SameSite`Indica se o navegador deve permitir que o cookie a ser anexado a solicitações do mesmo site ou entre sites. O padrão é `SameSiteMode.Lax`.

* `CookieBuilder.SecurePolicy`é um sinalizador que indica se o cookie criado deve ser limitado para o mesmo protocolo que a solicitação, HTTP ou HTTPS ou HTTPS. O padrão é `SameAsRequest`.

* `ExpireTimeSpan`é o `TimeSpan` depois que o cookie expira. Ele é adicionado para a data e hora atuais para criar a data de expiração para o cookie.

* `SlidingExpiration`é um sinalizador que indica se a data de expiração do cookie redefine quando houver mais de metade do `ExpireTimeSpan` do intervalo. A nova data de expiração é movida para frente para ser a data atual mais o `ExpireTimespan`. Um [tempo de expiração absoluta](xref:security/authentication/cookie#security-authentication-absolute-expiry) pode ser definida usando o `AuthenticationProperties` classe ao chamar `SignInAsync`. Uma expiração absoluta pode melhorar a segurança do seu aplicativo, limitando a quantidade de tempo para o qual o cookie de autenticação é válido.

Um exemplo de uso `CookieAuthenticationOptions` no `ConfigureServices` método *Startup.cs* segue:

```csharp
services.AddAuthentication()
        .AddCookie(options =>
        {
            options.Cookie.Name = "AuthCookie";
            options.Cookie.Domain = "contoso.com";
            options.Cookie.Path = "/";
            options.Cookie.HttpOnly = true;
            options.Cookie.SameSite = SameSiteMode.Lax;
            options.Cookie.SecurePolicy = CookieSecurePolicy.Always;
        });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

* `ClaimsIssuer`o emissor a ser usado para o [emissor](https://docs.microsoft.com/dotnet/api/system.security.claims.claim.issuer) propriedade em quaisquer declarações criado pelo middleware.

* `CookieDomain`é o nome de domínio ao qual o cookie é atendido. Por padrão, essa é a solicitação foi enviada para o nome do host. O navegador serve apenas o cookie para um nome de host correspondente. Talvez você queira ajustar isso para que os cookies disponíveis para qualquer host no seu domínio. Por exemplo, definir o domínio do cookie `.contoso.com` disponibiliza para `contoso.com`, `www.contoso.com`, `staging.www.contoso.com`, etc.

* `CookieHttpOnly`é um sinalizador que indica se o cookie deve ser acessível somente para servidores. O padrão é `true`. A alteração desse valor pode abrir o aplicativo roubo de cookies de seu aplicativo precisar de um bug de scripts entre sites.

* `CookiePath`pode ser usado para isolar aplicativos em execução no mesmo nome de host. Se você tiver um aplicativo em execução no `/app1` e para limitar os cookies emitidos para só será enviado para esse aplicativo, você deve definir o `CookiePath` propriedade `/app1`. Fazendo isso, o cookie só está disponível para solicitações para `/app1` ou algo abaixo dele.

* `CookieSecure`é um sinalizador que indica se o cookie criado deve ser limitado para o mesmo protocolo que a solicitação, HTTP ou HTTPS ou HTTPS. O padrão é `SameAsRequest`.

* `ExpireTimeSpan`é o `TimeSpan` depois que o cookie expira. Ele é adicionado para a data e hora atuais para criar a data de expiração para o cookie.

* `SlidingExpiration`é um sinalizador que indica se a data de expiração do cookie redefine quando houver mais de metade do `ExpireTimeSpan` do intervalo. A nova data de expiração é movida para frente para ser a data atual mais o `ExpireTimespan`. Um [tempo de expiração absoluta](xref:security/authentication/cookie#security-authentication-absolute-expiry) pode ser definida usando o `AuthenticationProperties` classe ao chamar `SignInAsync`. Uma expiração absoluta pode melhorar a segurança do seu aplicativo, limitando a quantidade de tempo para o qual o cookie de autenticação é válido.

Um exemplo de uso `CookieAuthenticationOptions` no `Configure` método *Startup.cs* segue:

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    CookieName = "AuthCookie",
    CookieDomain = "contoso.com",
    CookiePath = "/",
    CookieHttpOnly = true,
    CookieSecure = CookieSecurePolicy.Always
});
```

---

## <a name="persistent-cookies-and-absolute-expiry-times"></a>Cookies persistentes e tempos de expiração absoluta

Talvez você queira que a expiração do cookie para persistir nas sessões do navegador e quiser uma expiração absoluta para a identidade e o cookie transportando-lo. Essa persistência deve ser habilitada apenas com a permissão explícita do usuário, por meio de uma "Lembrar-Me" caixa de seleção no logon ou um mecanismo semelhante. Você pode executar essas ações usando o `AuthenticationProperties` parâmetro o `SignInAsync` método chamado quando [assinatura em uma identidade e criar o cookie](xref:security/authentication/cookie#security-authentication-cookie-middleware-creating-a-cookie). Por exemplo:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    "MyCookieAuthenticationScheme",
    principal,
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

O `AuthenticationProperties` reside classe, usada no trecho de código anterior, o `Microsoft.AspNetCore.Authentication` namespace.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync(
    "MyCookieAuthenticationScheme",
    principal,
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

O `AuthenticationProperties` reside classe, usada no trecho de código anterior, o `Microsoft.AspNetCore.Http.Authentication` namespace.

---

Trecho de código anterior cria uma identidade e o cookie correspondente que sobrevive a fechamentos de navegador. As configurações de expiração deslizante configuradas anteriormente pelo [opções de cookie](xref:security/authentication/cookie#security-authentication-cookie-options) são consideradas ainda. Se o cookie expira durante o navegador for fechado, o navegador a limpa depois que ele seja reiniciado.

<a name="security-authentication-absolute-expiry"></a>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    "MyCookieAuthenticationScheme",
    principal,
    new AuthenticationProperties
    {
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync(
    "MyCookieAuthenticationScheme",
    principal,
    new AuthenticationProperties
    {
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

---

Trecho de código anterior cria uma identidade e o cookie correspondente que tem duração de 20 minutos. Isso ignora as configurações de expiração deslizante configuradas anteriormente pelo [opções de cookie](xref:security/authentication/cookie#security-authentication-cookie-options).

O `ExpiresUtc` e `IsPersistent` propriedades são mutuamente exclusivas.
