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
ms.openlocfilehash: ea9c93e34a3242b5b3716404228edb8902baf625
ms.sourcegitcommit: e3b1726cc04e80dc28464c35259edbd3bc39a438
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/12/2017
---
# <a name="using-cookie-authentication-without-aspnet-core-identity"></a><span data-ttu-id="8c3ef-104">Usando a autenticação de Cookie sem identidade do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8c3ef-104">Using Cookie Authentication without ASP.NET Core Identity</span></span>

<a name="security-authentication-cookie-middleware"></a>

<span data-ttu-id="8c3ef-105">ASP.NET Core 1. x fornece o cookie [middleware](../../fundamentals/middleware.md#fundamentals-middleware) que serializa um objeto de usuário em um cookie criptografado e, em seguida, em solicitações subsequentes, valida o cookie recria a entidade de segurança e a atribui para a `HttpContext.User` propriedade .</span><span class="sxs-lookup"><span data-stu-id="8c3ef-105">ASP.NET Core 1.x provides cookie [middleware](../../fundamentals/middleware.md#fundamentals-middleware) which serializes a user principal into an encrypted cookie and then, on subsequent requests, validates the cookie, recreates the principal, and assigns it to the `HttpContext.User` property.</span></span> <span data-ttu-id="8c3ef-106">Se você quiser fornecer suas próprias telas de logon e bancos de dados de usuário, você pode usar o middleware de cookie como um recurso autônomo.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-106">If you want to provide your own login screens and user databases, you can use the cookie middleware as a standalone feature.</span></span>

<span data-ttu-id="8c3ef-107">Uma alteração importante no núcleo do ASP.NET 2. x é que o middleware de cookie ausente.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-107">A major change in ASP.NET Core 2.x is that the cookie middleware is absent.</span></span> <span data-ttu-id="8c3ef-108">Em vez disso, o `UseAuthentication` invocação de método no `Configure` método *Startup.cs* adiciona o AuthenticationMiddleware que define o `HttpContext.User` propriedade.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-108">Instead, the `UseAuthentication` method invocation in the `Configure` method of *Startup.cs* adds the AuthenticationMiddleware which sets the `HttpContext.User` property.</span></span>

<a name="security-authentication-cookie-middleware-configuring"></a>

## <a name="adding-and-configuring"></a><span data-ttu-id="8c3ef-109">Adicionando e configurando</span><span class="sxs-lookup"><span data-stu-id="8c3ef-109">Adding and configuring</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8c3ef-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8c3ef-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="8c3ef-111">Conclua as seguintes etapas:</span><span class="sxs-lookup"><span data-stu-id="8c3ef-111">Complete the following steps:</span></span>

- <span data-ttu-id="8c3ef-112">Se não usar o `Microsoft.AspNetCore.All` [metapackage](xref:fundamentals/metapackage), instale a versão 2.0 + do `Microsoft.AspNetCore.Authentication.Cookies` pacote NuGet em seu projeto.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-112">If not using the `Microsoft.AspNetCore.All` [metapackage](xref:fundamentals/metapackage), install version 2.0+ of the `Microsoft.AspNetCore.Authentication.Cookies` NuGet package in your project.</span></span>

- <span data-ttu-id="8c3ef-113">Invocar o `UseAuthentication` método no `Configure` método o *Startup.cs* arquivo:</span><span class="sxs-lookup"><span data-stu-id="8c3ef-113">Invoke the `UseAuthentication` method in the `Configure` method of the *Startup.cs* file:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="8c3ef-114">Invocar o `AddAuthentication` e `AddCookie` métodos no `ConfigureServices` método o *Startup.cs* arquivo:</span><span class="sxs-lookup"><span data-stu-id="8c3ef-114">Invoke the `AddAuthentication` and `AddCookie` methods in the `ConfigureServices` method of the *Startup.cs* file:</span></span>

    ```csharp
    services.AddAuthentication("MyCookieAuthenticationScheme")
            .AddCookie("MyCookieAuthenticationScheme", options => {
                options.AccessDeniedPath = "/Account/Forbidden/";
                options.LoginPath = "/Account/Unauthorized/";
            });
    ```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8c3ef-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8c3ef-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="8c3ef-116">Conclua as seguintes etapas:</span><span class="sxs-lookup"><span data-stu-id="8c3ef-116">Complete the following steps:</span></span>

- <span data-ttu-id="8c3ef-117">Instalar o `Microsoft.AspNetCore.Authentication.Cookies` pacote NuGet em seu projeto.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-117">Install the `Microsoft.AspNetCore.Authentication.Cookies` NuGet package in your project.</span></span> <span data-ttu-id="8c3ef-118">Este pacote contém o middleware de cookie.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-118">This package contains the cookie middleware.</span></span>

- <span data-ttu-id="8c3ef-119">Adicione as seguintes linhas para o `Configure` método no seu *Startup.cs* arquivo antes do `app.UseMvc()` instrução:</span><span class="sxs-lookup"><span data-stu-id="8c3ef-119">Add the following lines to the `Configure` method in your *Startup.cs* file before the `app.UseMvc()` statement:</span></span>

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

<span data-ttu-id="8c3ef-120">Os trechos de código acima configurar algumas ou todas as seguintes opções:</span><span class="sxs-lookup"><span data-stu-id="8c3ef-120">The code snippets above configure some or all of the following options:</span></span>

* <span data-ttu-id="8c3ef-121">`AccessDeniedPath`-Este é o caminho relativo para o qual solicitações redirecionam quando um usuário tenta acessar um recurso, mas não passar por nenhuma [diretivas de autorização](xref:security/authorization/policies#security-authorization-policies-based) para esse recurso.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-121">`AccessDeniedPath` - This is the relative path to which requests redirect when a user attempts to access a resource but does not pass any [authorization policies](xref:security/authorization/policies#security-authorization-policies-based) for that resource.</span></span>

* <span data-ttu-id="8c3ef-122">`AuthenticationScheme`-Este é um valor pelo qual um esquema de autenticação de cookie específico é conhecido.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-122">`AuthenticationScheme` - This is a value by which a particular cookie authentication scheme is known.</span></span> <span data-ttu-id="8c3ef-123">Isso é útil quando há várias instâncias de autenticação de cookie e o aplicativo precisa [limitar autorização para uma instância](xref:security/authorization/limitingidentitybyscheme).</span><span class="sxs-lookup"><span data-stu-id="8c3ef-123">This is useful when there are multiple instances of cookie authentication and the app needs to [limit authorization to one instance](xref:security/authorization/limitingidentitybyscheme).</span></span>

* <span data-ttu-id="8c3ef-124">`AutomaticAuthenticate`-Este sinalizador é relevante apenas para o ASP.NET Core 1. x.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-124">`AutomaticAuthenticate` - This flag is relevant only for ASP.NET Core 1.x.</span></span> <span data-ttu-id="8c3ef-125">Isso indica que a autenticação de cookie deve executar em cada solicitação e tente validar e reconstrua qualquer entidade de segurança serializada criado por ele.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-125">It indicates that the cookie authentication should run on every request and attempt to validate and reconstruct any serialized principal it created.</span></span>

* <span data-ttu-id="8c3ef-126">`AutomaticChallenge`-Este sinalizador é relevante apenas para o ASP.NET Core 1. x.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-126">`AutomaticChallenge` - This flag is relevant only for ASP.NET Core 1.x.</span></span> <span data-ttu-id="8c3ef-127">Indica que a autenticação de cookie 1. x deve redirecionar o navegador para o `LoginPath` ou `AccessDeniedPath` quando ocorre falha na autorização.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-127">It indicates that the 1.x cookie authentication should redirect the browser to the `LoginPath` or `AccessDeniedPath` when authorization fails.</span></span>

* <span data-ttu-id="8c3ef-128">`LoginPath`-Este é o caminho relativo para o qual solicitações redirecionam quando um usuário tenta acessar um recurso, mas não foi autenticado.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-128">`LoginPath` - This is the relative path to which requests redirect when a user attempts to access a resource but has not been authenticated.</span></span>

<span data-ttu-id="8c3ef-129">[Outras opções](xref:security/authentication/cookie#security-authentication-cookie-options) incluem a capacidade de definir o emissor para quaisquer declarações que cria a autenticação de cookie, o nome do cookie de autenticação for removido, o domínio para o cookie e várias propriedades de segurança no cookie.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-129">[Other options](xref:security/authentication/cookie#security-authentication-cookie-options) include the ability to set the issuer for any claims the cookie authentication creates, the name of the cookie the authentication drops, the domain for the cookie and various security properties on the cookie.</span></span> <span data-ttu-id="8c3ef-130">Por padrão, a autenticação de cookie usa opções de segurança apropriadas para os cookies que cria, tais como:</span><span class="sxs-lookup"><span data-stu-id="8c3ef-130">By default, the cookie authentication uses appropriate security options for any cookies it creates, such as:</span></span>
- <span data-ttu-id="8c3ef-131">A definição do sinalizador HttpOnly para impedir o acesso de cookie no JavaScript do lado do cliente</span><span class="sxs-lookup"><span data-stu-id="8c3ef-131">Setting the HttpOnly flag to prevent cookie access in client-side JavaScript</span></span>
- <span data-ttu-id="8c3ef-132">Limitando o cookie para HTTPS, se uma solicitação se mover sobre HTTPS</span><span class="sxs-lookup"><span data-stu-id="8c3ef-132">Limiting the cookie to HTTPS if a request has traveled over HTTPS</span></span>

<a name="security-authentication-cookie-middleware-creating-a-cookie"></a>

## <a name="creating-an-identity-cookie"></a><span data-ttu-id="8c3ef-133">Criando um cookie de identidade</span><span class="sxs-lookup"><span data-stu-id="8c3ef-133">Creating an Identity cookie</span></span>

<span data-ttu-id="8c3ef-134">Para criar um cookie que contém as informações do usuário, você precisa construir uma [ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal) contendo as informações que você deseja ser serializado no cookie.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-134">To create a cookie holding your user information, you must construct a [ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal) holding the information you wish to be serialized in the cookie.</span></span> <span data-ttu-id="8c3ef-135">Uma vez que um adequado `ClaimsPrincipal` de objeto, chame o seguinte dentro de seu método de controlador:</span><span class="sxs-lookup"><span data-stu-id="8c3ef-135">Once you have a suitable `ClaimsPrincipal` object, call the following inside your controller method:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8c3ef-136">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8c3ef-136">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync("MyCookieAuthenticationScheme", principal);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8c3ef-137">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8c3ef-137">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync("MyCookieAuthenticationScheme", principal);
```

---

<span data-ttu-id="8c3ef-138">Isso cria um cookie criptografado e o adiciona à resposta atual.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-138">This creates an encrypted cookie and adds it to the current response.</span></span> <span data-ttu-id="8c3ef-139">O `AuthenticationScheme` especificado durante a [configuração](xref:security/authentication/cookie#security-authentication-cookie-middleware-configuring) deve ser usado ao chamar `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-139">The `AuthenticationScheme` specified during [configuration](xref:security/authentication/cookie#security-authentication-cookie-middleware-configuring) must be used when calling `SignInAsync`.</span></span>

<span data-ttu-id="8c3ef-140">Nos bastidores, a criptografia é ASP.NET Core [proteção de dados](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) sistema.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-140">Under the covers, the encryption used is ASP.NET Core's [Data Protection](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) system.</span></span> <span data-ttu-id="8c3ef-141">Se você estiver hospedando em vários computadores, carga balanceamento ou usando uma web farm, em seguida, você precisa [configurar a proteção de dados](xref:security/data-protection/configuration/overview#data-protection-configuring) para usar o mesmo anel de chave e o identificador do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-141">If you are hosting on multiple machines, load balancing, or using a web farm, then you need to [configure data protection](xref:security/data-protection/configuration/overview#data-protection-configuring) to use the same key ring and application identifier.</span></span>

## <a name="signing-out"></a><span data-ttu-id="8c3ef-142">Sair</span><span class="sxs-lookup"><span data-stu-id="8c3ef-142">Signing out</span></span>

<span data-ttu-id="8c3ef-143">Para desconectar o usuário atual e exclua seus cookies, chame a seguir em seu controlador:</span><span class="sxs-lookup"><span data-stu-id="8c3ef-143">To sign out the current user and delete their cookie, call the following inside your controller:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8c3ef-144">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8c3ef-144">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
await HttpContext.SignOutAsync("MyCookieAuthenticationScheme");
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8c3ef-145">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8c3ef-145">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignOutAsync("MyCookieAuthenticationScheme");
```

---

## <a name="reacting-to-back-end-changes"></a><span data-ttu-id="8c3ef-146">Reagir a alterações de back-end</span><span class="sxs-lookup"><span data-stu-id="8c3ef-146">Reacting to back-end changes</span></span>

>[!WARNING]
> <span data-ttu-id="8c3ef-147">Quando um cookie principal tiver sido criado, ele se torna a única fonte de identidade.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-147">Once a principal cookie has been created, it becomes the single source of identity.</span></span> <span data-ttu-id="8c3ef-148">Mesmo se você desabilitar um usuário nos seus sistemas de back-end, a autenticação de cookie não tem conhecimento sobre isso e um usuário permanece conectado como o cookie é válido.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-148">Even if you disable a user in your back-end systems, the cookie authentication has no knowledge of this, and a user stays logged in as long as their cookie is valid.</span></span>

<span data-ttu-id="8c3ef-149">A autenticação de cookie fornece uma série de eventos em sua classe de opção.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-149">The cookie authentication provides a series of events in its option class.</span></span> <span data-ttu-id="8c3ef-150">O `ValidateAsync()` evento pode ser usado para interceptar e ignorar a validação da identidade do cookie.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-150">The `ValidateAsync()` event can be used to intercept and override validation of the cookie identity.</span></span>

<span data-ttu-id="8c3ef-151">Considere um banco de dados de usuário de back-end que pode ter uma coluna de "LastChanged".</span><span class="sxs-lookup"><span data-stu-id="8c3ef-151">Consider a back-end user database that may have a "LastChanged" column.</span></span> <span data-ttu-id="8c3ef-152">Para invalidar um cookie quando o banco de dados é alterado, você deve primeiro, quando [criar o cookie](xref:security/authentication/cookie#security-authentication-cookie-middleware-creating-a-cookie), adicione uma declaração de "LastChanged" que contém o valor atual.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-152">In order to invalidate a cookie when the database changes, you should first, when [creating the cookie](xref:security/authentication/cookie#security-authentication-cookie-middleware-creating-a-cookie), add a "LastChanged" claim containing the current value.</span></span> <span data-ttu-id="8c3ef-153">Quando o banco de dados for alterado, o valor de "LastChanged" deve ser atualizado.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-153">When the database changes, the "LastChanged" value should be updated.</span></span>

<span data-ttu-id="8c3ef-154">Para implementar uma substituição para o `ValidateAsync()` evento, você deve escrever um método com a seguinte assinatura:</span><span class="sxs-lookup"><span data-stu-id="8c3ef-154">To implement an override for the `ValidateAsync()` event, you must write a method with the following signature:</span></span>

```csharp
Task ValidateAsync(CookieValidatePrincipalContext context);
```

<span data-ttu-id="8c3ef-155">A identidade do ASP.NET Core implementa essa verificação como parte de seu `SecurityStampValidator`.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-155">ASP.NET Core Identity implements this check as part of its `SecurityStampValidator`.</span></span> <span data-ttu-id="8c3ef-156">Um exemplo é semelhante ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="8c3ef-156">An example looks like the following:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8c3ef-157">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8c3ef-157">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

<span data-ttu-id="8c3ef-158">Isso seria, em seguida, ser ligado durante o registro do serviço de cookie no `ConfigureServices` método *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="8c3ef-158">This would then be wired up during cookie service registration in the `ConfigureServices` method of *Startup.cs*:</span></span>

```csharp
services.AddAuthentication("MyCookieAuthenticationScheme")
        .AddCookie("MyCookieAuthenticationScheme", options =>
        {
            options.Events = new CookieAuthenticationEvents
            {
                OnValidatePrincipal = LastChangedValidator.ValidateAsync
            };
        });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8c3ef-159">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8c3ef-159">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

<span data-ttu-id="8c3ef-160">Isso seria, em seguida, ser ligado durante a configuração de autenticação de cookie no `Configure` método *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="8c3ef-160">This would then be wired up during cookie authentication configuration in the `Configure` method of *Startup.cs*:</span></span>

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

<span data-ttu-id="8c3ef-161">Considere o exemplo no qual seu nome foi atualizado &mdash; uma decisão que não afeta a segurança de qualquer forma.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-161">Consider the example in which their name has been updated &mdash; a decision which doesn't affect security in any way.</span></span> <span data-ttu-id="8c3ef-162">Se você quiser atualizar não destrutivo a entidade de usuário, você pode chamar `context.ReplacePrincipal()` e defina o `context.ShouldRenew` propriedade `true`.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-162">If you want to non-destructively update the user principal, you can call `context.ReplacePrincipal()` and set the `context.ShouldRenew` property to `true`.</span></span>

<a name="security-authentication-cookie-options"></a>

## <a name="controlling-cookie-options"></a><span data-ttu-id="8c3ef-163">Controlando opções de cookie</span><span class="sxs-lookup"><span data-stu-id="8c3ef-163">Controlling cookie options</span></span>

<span data-ttu-id="8c3ef-164">O [CookieAuthenticationOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) classe vem com várias opções de configuração para ajustar os cookies que está sendo criados.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-164">The [CookieAuthenticationOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) class comes with various configuration options to fine-tune the cookies being created.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8c3ef-165">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8c3ef-165">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="8c3ef-166">2. x unifica as APIs usadas para configurar os cookies do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-166">ASP.NET Core 2.x unifies the APIs used for configuring cookies.</span></span> <span data-ttu-id="8c3ef-167">1. x que APIs foram marcados como obsoletos e um novo `Cookie` propriedade do tipo `CookieBuilder` foi introduzido no `CookieAuthenticationOptions` classe.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-167">The 1.x APIs have been marked as obsolete, and a new `Cookie` property of type `CookieBuilder` has been introduced in the `CookieAuthenticationOptions` class.</span></span> <span data-ttu-id="8c3ef-168">É recomendável que você migre para APIs 2. x.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-168">It's recommended that you migrate to the 2.x APIs.</span></span>

* <span data-ttu-id="8c3ef-169">`ClaimsIssuer`o emissor a ser usado para o [emissor](https://docs.microsoft.com/dotnet/api/system.security.claims.claim.issuer) propriedade em quaisquer declarações criados pela autenticação de cookie.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-169">`ClaimsIssuer` is the issuer to be used for the [Issuer](https://docs.microsoft.com/dotnet/api/system.security.claims.claim.issuer) property on any claims created by the cookie authentication.</span></span>

* <span data-ttu-id="8c3ef-170">`CookieBuilder.Domain`é o nome de domínio ao qual o cookie é atendido.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-170">`CookieBuilder.Domain` is the domain name to which the cookie is served.</span></span> <span data-ttu-id="8c3ef-171">Por padrão, essa é a solicitação foi enviada para o nome do host.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-171">By default, this is the host name the request was sent to.</span></span> <span data-ttu-id="8c3ef-172">O navegador serve apenas o cookie para um nome de host correspondente.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-172">The browser only serves the cookie to a matching host name.</span></span> <span data-ttu-id="8c3ef-173">Talvez você queira ajustar isso para que os cookies disponíveis para qualquer host no seu domínio.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-173">You may wish to adjust this to have cookies available to any host in your domain.</span></span> <span data-ttu-id="8c3ef-174">Por exemplo, definir o domínio do cookie `.contoso.com` disponibiliza para `contoso.com`, `www.contoso.com`, `staging.www.contoso.com`, etc.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-174">For example, setting the cookie domain to `.contoso.com` makes it available to `contoso.com`, `www.contoso.com`, `staging.www.contoso.com`, etc.</span></span>

* <span data-ttu-id="8c3ef-175">`CookieBuilder.HttpOnly`é um sinalizador que indica se o cookie deve ser acessível somente para servidores.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-175">`CookieBuilder.HttpOnly` is a flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="8c3ef-176">O padrão é `true`.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-176">This defaults to `true`.</span></span> <span data-ttu-id="8c3ef-177">A alteração desse valor pode abrir o aplicativo roubo de cookies de seu aplicativo precisar de um bug de scripts entre sites.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-177">Changing this value may open your application to cookie theft should your application have a Cross-Site Scripting bug.</span></span>

* <span data-ttu-id="8c3ef-178">`CookieBuilder.Path`pode ser usado para isolar aplicativos em execução no mesmo nome de host.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-178">`CookieBuilder.Path` can be used to isolate applications running on the same host name.</span></span> <span data-ttu-id="8c3ef-179">Se você tiver um aplicativo em execução no `/app1` e para limitar os cookies emitidos para só será enviado para esse aplicativo, você deve definir o `CookiePath` propriedade `/app1`.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-179">If you have an app running in `/app1` and want to limit the cookies issued to just be sent to that application, then you should set the `CookiePath` property to `/app1`.</span></span> <span data-ttu-id="8c3ef-180">Fazendo isso, o cookie só está disponível para solicitações para `/app1` ou algo abaixo dele.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-180">By doing so, the cookie is only available to requests to `/app1` or anything underneath it.</span></span>

* <span data-ttu-id="8c3ef-181">`CookieBuilder.SameSite`Indica se o navegador deve permitir que o cookie a ser anexado a solicitações do mesmo site ou entre sites.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-181">`CookieBuilder.SameSite` indicates whether the browser should allow the cookie to be attached to same-site or cross-site requests.</span></span> <span data-ttu-id="8c3ef-182">O padrão é `SameSiteMode.Lax`.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-182">This defaults to `SameSiteMode.Lax`.</span></span>

* <span data-ttu-id="8c3ef-183">`CookieBuilder.SecurePolicy`é um sinalizador que indica se o cookie criado deve ser limitado para o mesmo protocolo que a solicitação, HTTP ou HTTPS ou HTTPS.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-183">`CookieBuilder.SecurePolicy` is a flag indicating if the cookie created should be limited to HTTPS, HTTP or HTTPS, or the same protocol as the request.</span></span> <span data-ttu-id="8c3ef-184">O padrão é `SameAsRequest`.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-184">This defaults to `SameAsRequest`.</span></span>

* <span data-ttu-id="8c3ef-185">`ExpireTimeSpan`é o `TimeSpan` depois que o cookie expira.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-185">`ExpireTimeSpan` is the `TimeSpan` after which the cookie expires.</span></span> <span data-ttu-id="8c3ef-186">Ele é adicionado para a data e hora atuais para criar a data de expiração para o cookie.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-186">It's added to the current date and time to create the expiry date for the cookie.</span></span>

* <span data-ttu-id="8c3ef-187">`SlidingExpiration`é um sinalizador que indica se a data de expiração do cookie redefine quando houver mais de metade do `ExpireTimeSpan` do intervalo.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-187">`SlidingExpiration` is a flag indicating whether the cookie expiration date resets when more than half of the `ExpireTimeSpan` interval has passed.</span></span> <span data-ttu-id="8c3ef-188">A nova data de expiração é movida para frente para ser a data atual mais o `ExpireTimespan`.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-188">The new expiry date is moved forward to be the current date plus the `ExpireTimespan`.</span></span> <span data-ttu-id="8c3ef-189">Um [tempo de expiração absoluta](xref:security/authentication/cookie#security-authentication-absolute-expiry) pode ser definida usando o `AuthenticationProperties` classe ao chamar `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-189">An [absolute expiry time](xref:security/authentication/cookie#security-authentication-absolute-expiry) can be set by using the `AuthenticationProperties` class when calling `SignInAsync`.</span></span> <span data-ttu-id="8c3ef-190">Uma expiração absoluta pode melhorar a segurança do seu aplicativo, limitando a quantidade de tempo para o qual o cookie de autenticação é válido.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-190">An absolute expiry can improve the security of your application by limiting the amount of time for which the authentication cookie is valid.</span></span>

<span data-ttu-id="8c3ef-191">Um exemplo de uso `CookieAuthenticationOptions` no `ConfigureServices` método *Startup.cs* segue:</span><span class="sxs-lookup"><span data-stu-id="8c3ef-191">An example of using `CookieAuthenticationOptions` in the `ConfigureServices` method of *Startup.cs* follows:</span></span>

```csharp
services.AddAuthentication("MyCookieAuthenticationScheme")
        .AddCookie("MyCookieAuthenticationScheme", options =>
        {
            options.Cookie.Name = "AuthCookie";
            options.Cookie.Domain = "contoso.com";
            options.Cookie.Path = "/";
            options.Cookie.HttpOnly = true;
            options.Cookie.SameSite = SameSiteMode.Lax;
            options.Cookie.SecurePolicy = CookieSecurePolicy.Always;
        });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8c3ef-192">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8c3ef-192">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* <span data-ttu-id="8c3ef-193">`ClaimsIssuer`o emissor a ser usado para o [emissor](https://docs.microsoft.com/dotnet/api/system.security.claims.claim.issuer) propriedade em quaisquer declarações criado pelo middleware.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-193">`ClaimsIssuer` is the issuer to be used for the [Issuer](https://docs.microsoft.com/dotnet/api/system.security.claims.claim.issuer) property on any claims created by the middleware.</span></span>

* <span data-ttu-id="8c3ef-194">`CookieDomain`é o nome de domínio ao qual o cookie é atendido.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-194">`CookieDomain` is the domain name to which the cookie is served.</span></span> <span data-ttu-id="8c3ef-195">Por padrão, essa é a solicitação foi enviada para o nome do host.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-195">By default, this is the host name the request was sent to.</span></span> <span data-ttu-id="8c3ef-196">O navegador serve apenas o cookie para um nome de host correspondente.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-196">The browser only serves the cookie to a matching host name.</span></span> <span data-ttu-id="8c3ef-197">Talvez você queira ajustar isso para que os cookies disponíveis para qualquer host no seu domínio.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-197">You may wish to adjust this to have cookies available to any host in your domain.</span></span> <span data-ttu-id="8c3ef-198">Por exemplo, definir o domínio do cookie `.contoso.com` disponibiliza para `contoso.com`, `www.contoso.com`, `staging.www.contoso.com`, etc.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-198">For example, setting the cookie domain to `.contoso.com` makes it available to `contoso.com`, `www.contoso.com`, `staging.www.contoso.com`, etc.</span></span>

* <span data-ttu-id="8c3ef-199">`CookieHttpOnly`é um sinalizador que indica se o cookie deve ser acessível somente para servidores.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-199">`CookieHttpOnly` is a flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="8c3ef-200">O padrão é `true`.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-200">This defaults to `true`.</span></span> <span data-ttu-id="8c3ef-201">A alteração desse valor pode abrir o aplicativo roubo de cookies de seu aplicativo precisar de um bug de scripts entre sites.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-201">Changing this value may open your application to cookie theft should your application have a Cross-Site Scripting bug.</span></span>

* <span data-ttu-id="8c3ef-202">`CookiePath`pode ser usado para isolar aplicativos em execução no mesmo nome de host.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-202">`CookiePath` can be used to isolate applications running on the same host name.</span></span> <span data-ttu-id="8c3ef-203">Se você tiver um aplicativo em execução no `/app1` e para limitar os cookies emitidos para só será enviado para esse aplicativo, você deve definir o `CookiePath` propriedade `/app1`.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-203">If you have an app running in `/app1` and want to limit the cookies issued to just be sent to that application, then you should set the `CookiePath` property to `/app1`.</span></span> <span data-ttu-id="8c3ef-204">Fazendo isso, o cookie só está disponível para solicitações para `/app1` ou algo abaixo dele.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-204">By doing so, the cookie is only available to requests to `/app1` or anything underneath it.</span></span>

* <span data-ttu-id="8c3ef-205">`CookieSecure`é um sinalizador que indica se o cookie criado deve ser limitado para o mesmo protocolo que a solicitação, HTTP ou HTTPS ou HTTPS.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-205">`CookieSecure` is a flag indicating if the cookie created should be limited to HTTPS, HTTP or HTTPS, or the same protocol as the request.</span></span> <span data-ttu-id="8c3ef-206">O padrão é `SameAsRequest`.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-206">This defaults to `SameAsRequest`.</span></span>

* <span data-ttu-id="8c3ef-207">`ExpireTimeSpan`é o `TimeSpan` depois que o cookie expira.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-207">`ExpireTimeSpan` is the `TimeSpan` after which the cookie expires.</span></span> <span data-ttu-id="8c3ef-208">Ele é adicionado para a data e hora atuais para criar a data de expiração para o cookie.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-208">It's added to the current date and time to create the expiry date for the cookie.</span></span>

* <span data-ttu-id="8c3ef-209">`SlidingExpiration`é um sinalizador que indica se a data de expiração do cookie redefine quando houver mais de metade do `ExpireTimeSpan` do intervalo.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-209">`SlidingExpiration` is a flag indicating whether the cookie expiration date resets when more than half of the `ExpireTimeSpan` interval has passed.</span></span> <span data-ttu-id="8c3ef-210">A nova data de expiração é movida para frente para ser a data atual mais o `ExpireTimespan`.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-210">The new expiry date is moved forward to be the current date plus the `ExpireTimespan`.</span></span> <span data-ttu-id="8c3ef-211">Um [tempo de expiração absoluta](xref:security/authentication/cookie#security-authentication-absolute-expiry) pode ser definida usando o `AuthenticationProperties` classe ao chamar `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-211">An [absolute expiry time](xref:security/authentication/cookie#security-authentication-absolute-expiry) can be set by using the `AuthenticationProperties` class when calling `SignInAsync`.</span></span> <span data-ttu-id="8c3ef-212">Uma expiração absoluta pode melhorar a segurança do seu aplicativo, limitando a quantidade de tempo para o qual o cookie de autenticação é válido.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-212">An absolute expiry can improve the security of your application by limiting the amount of time for which the authentication cookie is valid.</span></span>

<span data-ttu-id="8c3ef-213">Um exemplo de uso `CookieAuthenticationOptions` no `Configure` método *Startup.cs* segue:</span><span class="sxs-lookup"><span data-stu-id="8c3ef-213">An example of using `CookieAuthenticationOptions` in the `Configure` method of *Startup.cs* follows:</span></span>

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

## <a name="persistent-cookies-and-absolute-expiry-times"></a><span data-ttu-id="8c3ef-214">Cookies persistentes e tempos de expiração absoluta</span><span class="sxs-lookup"><span data-stu-id="8c3ef-214">Persistent cookies and absolute expiry times</span></span>

<span data-ttu-id="8c3ef-215">Talvez você queira que a expiração do cookie para persistir nas sessões do navegador e quiser uma expiração absoluta para a identidade e o cookie transportando-lo.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-215">You may want the cookie expiry to persist across browser sessions and want an absolute expiry to the identity and the cookie transporting it.</span></span> <span data-ttu-id="8c3ef-216">Essa persistência deve ser habilitada apenas com a permissão explícita do usuário, por meio de uma "Lembrar-Me" caixa de seleção no logon ou um mecanismo semelhante.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-216">This persistence should only be enabled with explicit user consent, via a "Remember Me" checkbox on login or a similar mechanism.</span></span> <span data-ttu-id="8c3ef-217">Você pode executar essas ações usando o `AuthenticationProperties` parâmetro o `SignInAsync` método chamado quando [assinatura em uma identidade e criar o cookie](xref:security/authentication/cookie#security-authentication-cookie-middleware-creating-a-cookie).</span><span class="sxs-lookup"><span data-stu-id="8c3ef-217">You can do these things by using the `AuthenticationProperties` parameter on the `SignInAsync` method called when [signing in an identity and creating the cookie](xref:security/authentication/cookie#security-authentication-cookie-middleware-creating-a-cookie).</span></span> <span data-ttu-id="8c3ef-218">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="8c3ef-218">For example:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8c3ef-219">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8c3ef-219">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    "MyCookieAuthenticationScheme",
    principal,
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

<span data-ttu-id="8c3ef-220">O `AuthenticationProperties` reside classe, usada no trecho de código anterior, o `Microsoft.AspNetCore.Authentication` namespace.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-220">The `AuthenticationProperties` class, used in the preceding code snippet, resides in the `Microsoft.AspNetCore.Authentication` namespace.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8c3ef-221">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8c3ef-221">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync(
    "MyCookieAuthenticationScheme",
    principal,
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

<span data-ttu-id="8c3ef-222">O `AuthenticationProperties` reside classe, usada no trecho de código anterior, o `Microsoft.AspNetCore.Http.Authentication` namespace.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-222">The `AuthenticationProperties` class, used in the preceding code snippet, resides in the `Microsoft.AspNetCore.Http.Authentication` namespace.</span></span>

---

<span data-ttu-id="8c3ef-223">Trecho de código anterior cria uma identidade e o cookie correspondente que sobrevive a fechamentos de navegador.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-223">The preceding code snippet creates an identity and corresponding cookie which survives through browser closures.</span></span> <span data-ttu-id="8c3ef-224">As configurações de expiração deslizante configuradas anteriormente pelo [opções de cookie](xref:security/authentication/cookie#security-authentication-cookie-options) são consideradas ainda.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-224">Any sliding expiration settings previously configured via [cookie options](xref:security/authentication/cookie#security-authentication-cookie-options) are still honored.</span></span> <span data-ttu-id="8c3ef-225">Se o cookie expira durante o navegador for fechado, o navegador a limpa depois que ele seja reiniciado.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-225">If the cookie expires whilst the browser is closed, the browser clears it once it is restarted.</span></span>

<a name="security-authentication-absolute-expiry"></a>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8c3ef-226">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8c3ef-226">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    "MyCookieAuthenticationScheme",
    principal,
    new AuthenticationProperties
    {
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8c3ef-227">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8c3ef-227">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

<span data-ttu-id="8c3ef-228">Trecho de código anterior cria uma identidade e o cookie correspondente que tem duração de 20 minutos.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-228">The preceding code snippet creates an identity and corresponding cookie which lasts for 20 minutes.</span></span> <span data-ttu-id="8c3ef-229">Isso ignora as configurações de expiração deslizante configuradas anteriormente pelo [opções de cookie](xref:security/authentication/cookie#security-authentication-cookie-options).</span><span class="sxs-lookup"><span data-stu-id="8c3ef-229">This ignores any sliding expiration settings previously configured via [cookie options](xref:security/authentication/cookie#security-authentication-cookie-options).</span></span>

<span data-ttu-id="8c3ef-230">O `ExpiresUtc` e `IsPersistent` propriedades são mutuamente exclusivas.</span><span class="sxs-lookup"><span data-stu-id="8c3ef-230">The `ExpiresUtc` and `IsPersistent` properties are mutually exclusive.</span></span>
