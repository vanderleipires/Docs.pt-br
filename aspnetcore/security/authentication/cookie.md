---
title: "Usando a autenticação de Cookie sem identidade do ASP.NET Core"
author: rick-anderson
description: "Obter uma explicação de como usar a autenticação de cookie sem a identidade do ASP.NET Core"
keywords: ASP.NET Core, cookies
ms.author: riande
manager: wpickett
ms.date: 10/11/2017
ms.topic: article
ms.assetid: 2bdcbf95-8d9d-4537-a4a0-a5ee439dcb62
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/cookie
ms.openlocfilehash: ee660667251ec4a64f2b3e83f39214e9defcea03
ms.sourcegitcommit: 2d23ea501e0213bbacf65298acf1c8bd17209540
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/09/2018
---
# <a name="using-cookie-authentication-without-aspnet-core-identity"></a><span data-ttu-id="da2a2-104">Usando a autenticação de Cookie sem identidade do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="da2a2-104">Using Cookie Authentication without ASP.NET Core Identity</span></span>

<span data-ttu-id="da2a2-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="da2a2-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="da2a2-106">Como você viu nos tópicos anteriores de autenticação, [a identidade do ASP.NET Core](xref:security/authentication/identity) é um provedor de autenticação completa e completo para criar e manter os logons.</span><span class="sxs-lookup"><span data-stu-id="da2a2-106">As you've seen in the earlier authentication topics, [ASP.NET Core Identity](xref:security/authentication/identity) is a complete, full-featured authentication provider for creating and maintaining logins.</span></span> <span data-ttu-id="da2a2-107">No entanto, você talvez queira usar sua própria lógica de autenticação personalizada com autenticação baseada em cookie às vezes.</span><span class="sxs-lookup"><span data-stu-id="da2a2-107">However, you may want to use your own custom authentication logic with cookie-based authentication at times.</span></span> <span data-ttu-id="da2a2-108">Você pode usar a autenticação baseada em cookie como um provedor de autenticação autônomo sem a identidade do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="da2a2-108">You can use cookie-based authentication as a standalone authentication provider without ASP.NET Core Identity.</span></span>

<span data-ttu-id="da2a2-109">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/cookie/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="da2a2-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/cookie/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="da2a2-110">Para obter informações sobre a autenticação baseada em cookie Migrando do ASP.NET Core 1. x para 2.0, consulte [migrando autenticação e identidade de tópico do ASP.NET Core 2.0 (autenticação baseada em Cookie)](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication).</span><span class="sxs-lookup"><span data-stu-id="da2a2-110">For information on migrating cookie-based authentication from ASP.NET Core 1.x to 2.0, see [Migrating Authentication and Identity to ASP.NET Core 2.0 topic (Cookie-based Authentication)](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication).</span></span>

## <a name="configuration"></a><span data-ttu-id="da2a2-111">Configuração</span><span class="sxs-lookup"><span data-stu-id="da2a2-111">Configuration</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="da2a2-112">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="da2a2-112">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="da2a2-113">Se você não estiver usando o [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), instale a versão 2.0 + o [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) pacote NuGet.</span><span class="sxs-lookup"><span data-stu-id="da2a2-113">If you aren't using the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), install version 2.0+ of the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) NuGet package.</span></span>

<span data-ttu-id="da2a2-114">No `ConfigureServices` método, criar o serviço de Middleware de autenticação com o `AddAuthentication` e `AddCookie` métodos:</span><span class="sxs-lookup"><span data-stu-id="da2a2-114">In the `ConfigureServices` method, create the Authentication Middleware service with the `AddAuthentication` and `AddCookie` methods:</span></span>

[!code-csharp[Main](cookie/sample/Startup.cs?name=snippet1)]

<span data-ttu-id="da2a2-115">`AuthenticationScheme`passado para `AddAuthentication` define o esquema de autenticação padrão para o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="da2a2-115">`AuthenticationScheme` passed to `AddAuthentication` sets the default authentication scheme for the app.</span></span> <span data-ttu-id="da2a2-116">`AuthenticationScheme`é útil quando há várias instâncias de autenticação de cookie e você deseja [autorizar com um esquema específico](xref:security/authorization/limitingidentitybyscheme).</span><span class="sxs-lookup"><span data-stu-id="da2a2-116">`AuthenticationScheme` is useful when there are multiple instances of cookie authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="da2a2-117">Definindo o `AuthenticationScheme` para `CookieAuthenticationDefaults.AuthenticationScheme` fornece um valor de "Cookies" para o esquema.</span><span class="sxs-lookup"><span data-stu-id="da2a2-117">Setting the `AuthenticationScheme` to `CookieAuthenticationDefaults.AuthenticationScheme` provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="da2a2-118">Você pode fornecer qualquer valor de cadeia de caracteres que diferencia o esquema.</span><span class="sxs-lookup"><span data-stu-id="da2a2-118">You can supply any string value that distinguishes the scheme.</span></span>

<span data-ttu-id="da2a2-119">No `Configure` método, use o `UseAuthentication` método a invocar o Middleware de autenticação que define o `HttpContext.User` propriedade.</span><span class="sxs-lookup"><span data-stu-id="da2a2-119">In the `Configure` method, use the `UseAuthentication` method to invoke the Authentication Middleware that sets the `HttpContext.User` property.</span></span> <span data-ttu-id="da2a2-120">Chamar o `UseAuthentication` método antes de chamar `UseMvcWithDefaultRoute` ou `UseMvc`:</span><span class="sxs-lookup"><span data-stu-id="da2a2-120">Call the `UseAuthentication` method before calling `UseMvcWithDefaultRoute` or `UseMvc`:</span></span>

[!code-csharp[Main](cookie/sample/Startup.cs?name=snippet2)]

<span data-ttu-id="da2a2-121">**Opções de AddCookie**</span><span class="sxs-lookup"><span data-stu-id="da2a2-121">**AddCookie Options**</span></span>

<span data-ttu-id="da2a2-122">O [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0) classe é usada para configurar as opções de provedor de autenticação.</span><span class="sxs-lookup"><span data-stu-id="da2a2-122">The [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0) class is used to configure the authentication provider options.</span></span>

| <span data-ttu-id="da2a2-123">Opção</span><span class="sxs-lookup"><span data-stu-id="da2a2-123">Option</span></span> | <span data-ttu-id="da2a2-124">Descrição</span><span class="sxs-lookup"><span data-stu-id="da2a2-124">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="da2a2-125">AccessDeniedPath</span><span class="sxs-lookup"><span data-stu-id="da2a2-125">AccessDeniedPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath?view=aspnetcore-2.0) | <span data-ttu-id="da2a2-126">Fornece o caminho para fornecer um 302 não encontrado (redirecionamento de URL) quando disparadas por `HttpContext.ForbidAsync`.</span><span class="sxs-lookup"><span data-stu-id="da2a2-126">Provides the path to supply with a 302 Found (URL redirect) when triggered by `HttpContext.ForbidAsync`.</span></span> <span data-ttu-id="da2a2-127">O valor padrão é `/Account/AccessDenied`.</span><span class="sxs-lookup"><span data-stu-id="da2a2-127">The default value is `/Account/AccessDenied`.</span></span> |
| [<span data-ttu-id="da2a2-128">ClaimsIssuer</span><span class="sxs-lookup"><span data-stu-id="da2a2-128">ClaimsIssuer</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.claimsissuer?view=aspnetcore-2.0) | <span data-ttu-id="da2a2-129">O emissor a ser usado para o [emissor](/dotnet/api/system.security.claims.claim.issuer) propriedade em quaisquer declarações criado pelo serviço de autenticação de cookie.</span><span class="sxs-lookup"><span data-stu-id="da2a2-129">The issuer to use for the [Issuer](/dotnet/api/system.security.claims.claim.issuer) property on any claims created by the cookie authentication service.</span></span> |
| [<span data-ttu-id="da2a2-130">Cookie.Domain</span><span class="sxs-lookup"><span data-stu-id="da2a2-130">Cookie.Domain</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain?view=aspnetcore-2.0) | <span data-ttu-id="da2a2-131">O nome de domínio em que o cookie é atendido.</span><span class="sxs-lookup"><span data-stu-id="da2a2-131">The domain name where the cookie is served.</span></span> <span data-ttu-id="da2a2-132">Por padrão, esse é o nome do host da solicitação.</span><span class="sxs-lookup"><span data-stu-id="da2a2-132">By default, this is the host name of the request.</span></span> <span data-ttu-id="da2a2-133">O navegador envia somente o cookie em solicitações para um nome de host correspondente.</span><span class="sxs-lookup"><span data-stu-id="da2a2-133">The browser only sends the cookie in requests to a matching host name.</span></span> <span data-ttu-id="da2a2-134">Talvez você queira ajustar isso para que os cookies disponíveis para qualquer host no seu domínio.</span><span class="sxs-lookup"><span data-stu-id="da2a2-134">You may wish to adjust this to have cookies available to any host in your domain.</span></span> <span data-ttu-id="da2a2-135">Por exemplo, definir o domínio do cookie `.contoso.com` disponibiliza para `contoso.com`, `www.contoso.com`, e `staging.www.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="da2a2-135">For example, setting the cookie domain to `.contoso.com` makes it available to `contoso.com`, `www.contoso.com`, and `staging.www.contoso.com`.</span></span> |
| [<span data-ttu-id="da2a2-136">Cookie.Expiration</span><span class="sxs-lookup"><span data-stu-id="da2a2-136">Cookie.Expiration</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.expiration?view=aspnetcore-2.0) | <span data-ttu-id="da2a2-137">Obtém ou define o tempo de vida de um cookie.</span><span class="sxs-lookup"><span data-stu-id="da2a2-137">Gets or sets the lifespan of a cookie.</span></span> <span data-ttu-id="da2a2-138">No momento, essa opção não ops e se torna obsoleta no ASP.NET Core 2.1 +.</span><span class="sxs-lookup"><span data-stu-id="da2a2-138">Currently, this option no-ops and will become obsolete in ASP.NET Core 2.1+.</span></span> <span data-ttu-id="da2a2-139">Use o `ExpireTimeSpan` opção para definir a expiração do cookie.</span><span class="sxs-lookup"><span data-stu-id="da2a2-139">Use the `ExpireTimeSpan` option to set cookie expiration.</span></span> <span data-ttu-id="da2a2-140">Para obter mais informações, consulte [esclarecer o comportamento de CookieAuthenticationOptions.Cookie.Expiration](https://github.com/aspnet/Security/issues/1293).</span><span class="sxs-lookup"><span data-stu-id="da2a2-140">For more information, see [Clarify behavior of CookieAuthenticationOptions.Cookie.Expiration](https://github.com/aspnet/Security/issues/1293).</span></span> |
| [<span data-ttu-id="da2a2-141">Cookie.HttpOnly</span><span class="sxs-lookup"><span data-stu-id="da2a2-141">Cookie.HttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly?view=aspnetcore-2.0) | <span data-ttu-id="da2a2-142">Um sinalizador que indica se o cookie deve ser acessível somente para servidores.</span><span class="sxs-lookup"><span data-stu-id="da2a2-142">A flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="da2a2-143">A alteração desse valor para `false` permite que scripts do lado do cliente para acessar o cookie e pode abrir o aplicativo para roubo de cookie deve ter seu aplicativo um [script entre sites (XSS)](xref:security/cross-site-scripting) vulnerabilidade.</span><span class="sxs-lookup"><span data-stu-id="da2a2-143">Changing this value to `false` permits client-side scripts to access the cookie and may open your app to cookie theft should your app have a [Cross-site scripting (XSS)](xref:security/cross-site-scripting) vulnerability.</span></span> <span data-ttu-id="da2a2-144">O valor padrão é `true`.</span><span class="sxs-lookup"><span data-stu-id="da2a2-144">The default value is `true`.</span></span> |
| [<span data-ttu-id="da2a2-145">Cookie.Name</span><span class="sxs-lookup"><span data-stu-id="da2a2-145">Cookie.Name</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name?view=aspnetcore-2.0) | <span data-ttu-id="da2a2-146">Define o nome do cookie.</span><span class="sxs-lookup"><span data-stu-id="da2a2-146">Sets the name of the cookie.</span></span> |
| [<span data-ttu-id="da2a2-147">Cookie.Path</span><span class="sxs-lookup"><span data-stu-id="da2a2-147">Cookie.Path</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path?view=aspnetcore-2.0) | <span data-ttu-id="da2a2-148">Usado para isolar os aplicativos em execução no mesmo nome de host.</span><span class="sxs-lookup"><span data-stu-id="da2a2-148">Used to isolate apps running on the same host name.</span></span> <span data-ttu-id="da2a2-149">Se você tiver um aplicativo em execução no `/app1` e deseja restringir os cookies para esse aplicativo, defina o `CookiePath` propriedade `/app1`.</span><span class="sxs-lookup"><span data-stu-id="da2a2-149">If you have an app running at `/app1` and want to restrict cookies to that app, set the `CookiePath` property to `/app1`.</span></span> <span data-ttu-id="da2a2-150">Por isso, o cookie só está disponível em solicitações para `/app1` e qualquer aplicativo abaixo dele.</span><span class="sxs-lookup"><span data-stu-id="da2a2-150">By doing so, the cookie is only available on requests to `/app1` and any app underneath it.</span></span> |
| [<span data-ttu-id="da2a2-151">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="da2a2-151">Cookie.SameSite</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite?view=aspnetcore-2.0) | <span data-ttu-id="da2a2-152">Indica se o navegador deve permitir que o cookie a ser anexado ao mesmo site solicitações somente (`SameSiteMode.Strict`) ou solicitações de sites usando métodos HTTP seguros e solicitações do mesmo site (`SameSiteMode.Lax`).</span><span class="sxs-lookup"><span data-stu-id="da2a2-152">Indicates whether the browser should allow the cookie to be attached to same-site requests only (`SameSiteMode.Strict`) or cross-site requests using safe HTTP methods and same-site requests (`SameSiteMode.Lax`).</span></span> <span data-ttu-id="da2a2-153">Quando definido como `SameSiteMode.None`, o valor do cabeçalho de cookie não está definido.</span><span class="sxs-lookup"><span data-stu-id="da2a2-153">When set to `SameSiteMode.None`, the cookie header value isn't set.</span></span> <span data-ttu-id="da2a2-154">Observe que [Middleware do Cookie política](#cookie-policy-middleware) pode substituir o valor que você fornecer.</span><span class="sxs-lookup"><span data-stu-id="da2a2-154">Note that [Cookie Policy Middleware](#cookie-policy-middleware) might overwrite the value that you provide.</span></span> <span data-ttu-id="da2a2-155">Para dar suporte à autenticação OAuth, o valor padrão é `SameSiteMode.Lax`.</span><span class="sxs-lookup"><span data-stu-id="da2a2-155">To support OAuth authentication, the default value is `SameSiteMode.Lax`.</span></span> <span data-ttu-id="da2a2-156">Para obter mais informações, consulte [autenticação OAuth interrompida devido à política de cookie SameSite](https://github.com/aspnet/Security/issues/1231).</span><span class="sxs-lookup"><span data-stu-id="da2a2-156">For more information, see [OAuth authentication broken due to SameSite cookie policy](https://github.com/aspnet/Security/issues/1231).</span></span> |
| [<span data-ttu-id="da2a2-157">Cookie.SecurePolicy</span><span class="sxs-lookup"><span data-stu-id="da2a2-157">Cookie.SecurePolicy</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.securepolicy?view=aspnetcore-2.0) | <span data-ttu-id="da2a2-158">Um sinalizador que indica se o cookie criado deve ser limitado para HTTPS (`CookieSecurePolicy.Always`), HTTP ou HTTPS (`CookieSecurePolicy.None`), ou o mesmo protocolo que a solicitação (`CookieSecurePolicy.SameAsRequest`).</span><span class="sxs-lookup"><span data-stu-id="da2a2-158">A flag indicating if the cookie created should be limited to HTTPS (`CookieSecurePolicy.Always`), HTTP or HTTPS (`CookieSecurePolicy.None`), or the same protocol as the request (`CookieSecurePolicy.SameAsRequest`).</span></span> <span data-ttu-id="da2a2-159">O valor padrão é `CookieSecurePolicy.SameAsRequest`.</span><span class="sxs-lookup"><span data-stu-id="da2a2-159">The default value is `CookieSecurePolicy.SameAsRequest`.</span></span> |
| [<span data-ttu-id="da2a2-160">DataProtectionProvider</span><span class="sxs-lookup"><span data-stu-id="da2a2-160">DataProtectionProvider</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0) | <span data-ttu-id="da2a2-161">Conjuntos de `DataProtectionProvider` que é usado para criar o padrão `TicketDataFormat`.</span><span class="sxs-lookup"><span data-stu-id="da2a2-161">Sets the `DataProtectionProvider` that's used to create the default `TicketDataFormat`.</span></span> <span data-ttu-id="da2a2-162">Se o `TicketDataFormat` propriedade for definida, o `DataProtectionProvider` opção não é usada.</span><span class="sxs-lookup"><span data-stu-id="da2a2-162">If the `TicketDataFormat` property is set, the `DataProtectionProvider` option isn't used.</span></span> <span data-ttu-id="da2a2-163">Se não for fornecido, o provedor de proteção de dados do aplicativo padrão é usado.</span><span class="sxs-lookup"><span data-stu-id="da2a2-163">If not provided, the app's default data protection provider is used.</span></span> |
| [<span data-ttu-id="da2a2-164">Eventos</span><span class="sxs-lookup"><span data-stu-id="da2a2-164">Events</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.events?view=aspnetcore-2.0) | <span data-ttu-id="da2a2-165">O manipulador chama métodos no provedor que dê o controle de aplicativo em determinados pontos de processamento.</span><span class="sxs-lookup"><span data-stu-id="da2a2-165">The handler calls methods on the provider that give the app control at certain processing points.</span></span> <span data-ttu-id="da2a2-166">Se `Events` não fornecido, uma instância padrão é fornecida que não faz nada quando os métodos são chamados.</span><span class="sxs-lookup"><span data-stu-id="da2a2-166">If `Events` aren't provided, a default instance is supplied that does nothing when the methods are called.</span></span> |
| [<span data-ttu-id="da2a2-167">EventsType</span><span class="sxs-lookup"><span data-stu-id="da2a2-167">EventsType</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.eventstype?view=aspnetcore-2.0) | <span data-ttu-id="da2a2-168">Usado como o tipo de serviço para obter o `Events` instância em vez da propriedade.</span><span class="sxs-lookup"><span data-stu-id="da2a2-168">Used as the service type to get the `Events` instance instead of the property.</span></span> |
| [<span data-ttu-id="da2a2-169">ExpireTimeSpan</span><span class="sxs-lookup"><span data-stu-id="da2a2-169">ExpireTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.expiretimespan?view=aspnetcore-2.0) | <span data-ttu-id="da2a2-170">O `TimeSpan` depois que o tíquete de autenticação armazenado dentro o cookie expira.</span><span class="sxs-lookup"><span data-stu-id="da2a2-170">The `TimeSpan` after which the authentication ticket stored inside the cookie expires.</span></span> <span data-ttu-id="da2a2-171">`ExpireTimeSpan`é adicionado à hora atual para criar o tempo de expiração do tíquete de.</span><span class="sxs-lookup"><span data-stu-id="da2a2-171">`ExpireTimeSpan` is added to the current time to create the expiration time for the ticket.</span></span> <span data-ttu-id="da2a2-172">O `ExpiredTimeSpan` valor sempre vai para o AuthTicket criptografado verificado pelo servidor.</span><span class="sxs-lookup"><span data-stu-id="da2a2-172">The `ExpiredTimeSpan` value always goes into the encrypted AuthTicket verified by the server.</span></span> <span data-ttu-id="da2a2-173">Ele também pode ir para o [Set-Cookie](https://tools.ietf.org/html/rfc6265#section-4.1) cabeçalho, mas somente se `IsPersistent` está definido.</span><span class="sxs-lookup"><span data-stu-id="da2a2-173">It may also go into the [Set-Cookie](https://tools.ietf.org/html/rfc6265#section-4.1) header, but only if `IsPersistent` is set.</span></span> <span data-ttu-id="da2a2-174">Para definir `IsPersistent` para `true`, configure o [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties) passado para `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="da2a2-174">To set `IsPersistent` to `true`, configure the [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties) passed to `SignInAsync`.</span></span> <span data-ttu-id="da2a2-175">O valor padrão de `ExpireTimeSpan` é de 14 dias.</span><span class="sxs-lookup"><span data-stu-id="da2a2-175">The default value of `ExpireTimeSpan` is 14 days.</span></span> |
| [<span data-ttu-id="da2a2-176">LoginPath</span><span class="sxs-lookup"><span data-stu-id="da2a2-176">LoginPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath?view=aspnetcore-2.0) | <span data-ttu-id="da2a2-177">Fornece o caminho para fornecer um 302 não encontrado (redirecionamento de URL) quando disparadas por `HttpContext.ChallengeAsync`.</span><span class="sxs-lookup"><span data-stu-id="da2a2-177">Provides the path to supply with a 302 Found (URL redirect) when triggered by `HttpContext.ChallengeAsync`.</span></span> <span data-ttu-id="da2a2-178">A URL atual que gerou o 401 é adicionada para o `LoginPath` como um parâmetro de cadeia de caracteres de consulta nomeado pelo `ReturnUrlParameter`.</span><span class="sxs-lookup"><span data-stu-id="da2a2-178">The current URL that generated the 401 is added to the `LoginPath` as a query string parameter named by the `ReturnUrlParameter`.</span></span> <span data-ttu-id="da2a2-179">Uma vez uma solicitação para o `LoginPath` concede uma nova identidade, o `ReturnUrlParameter` valor é usado para redirecionar o navegador para a URL que fez com que o código de status não autorizado original.</span><span class="sxs-lookup"><span data-stu-id="da2a2-179">Once a request to the `LoginPath` grants a new sign-in identity, the `ReturnUrlParameter` value is used to redirect the browser back to the URL that caused the original unauthorized status code.</span></span> <span data-ttu-id="da2a2-180">O valor padrão é `/Account/Login`.</span><span class="sxs-lookup"><span data-stu-id="da2a2-180">The default value is `/Account/Login`.</span></span> |
| [<span data-ttu-id="da2a2-181">LogoutPath</span><span class="sxs-lookup"><span data-stu-id="da2a2-181">LogoutPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath?view=aspnetcore-2.0) | <span data-ttu-id="da2a2-182">Se o `LogoutPath` é fornecido para o manipulador, em seguida, redireciona uma solicitação para o caminho com base no valor da `ReturnUrlParameter`.</span><span class="sxs-lookup"><span data-stu-id="da2a2-182">If the `LogoutPath` is provided to the handler, then a request to that path redirects based on the value of the `ReturnUrlParameter`.</span></span> <span data-ttu-id="da2a2-183">O valor padrão é `/Account/Logout`.</span><span class="sxs-lookup"><span data-stu-id="da2a2-183">The default value is `/Account/Logout`.</span></span> |
| [<span data-ttu-id="da2a2-184">ReturnUrlParameter</span><span class="sxs-lookup"><span data-stu-id="da2a2-184">ReturnUrlParameter</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.returnurlparameter?view=aspnetcore-2.0) | <span data-ttu-id="da2a2-185">Determina o nome do parâmetro de cadeia de caracteres de consulta será anexado o manipulador de resposta 302 não encontrado (redirecionamento de URL).</span><span class="sxs-lookup"><span data-stu-id="da2a2-185">Determines the name of the query string parameter that's appended by the handler for a 302 Found (URL redirect) response.</span></span> <span data-ttu-id="da2a2-186">`ReturnUrlParameter`é usado quando uma solicitação chega no `LoginPath` ou `LogoutPath` para retornar o navegador a URL original depois que a ação de logon ou logout é executada.</span><span class="sxs-lookup"><span data-stu-id="da2a2-186">`ReturnUrlParameter` is used when a request arrives on the `LoginPath` or `LogoutPath` to return the browser to the original URL after the login or logout action is performed.</span></span> <span data-ttu-id="da2a2-187">O valor padrão é `ReturnUrl`.</span><span class="sxs-lookup"><span data-stu-id="da2a2-187">The default value is `ReturnUrl`.</span></span> |
| [<span data-ttu-id="da2a2-188">SessionStore</span><span class="sxs-lookup"><span data-stu-id="da2a2-188">SessionStore</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.sessionstore?view=aspnetcore-2.0) | <span data-ttu-id="da2a2-189">Um contêiner opcional usado para armazenar a identidade entre solicitações.</span><span class="sxs-lookup"><span data-stu-id="da2a2-189">An optional container used to store identity across requests.</span></span> <span data-ttu-id="da2a2-190">Quando usado, um identificador de sessão é enviado ao cliente.</span><span class="sxs-lookup"><span data-stu-id="da2a2-190">When used, only a session identifier is sent to the client.</span></span> <span data-ttu-id="da2a2-191">`SessionStore`pode ser usado para atenuar problemas potenciais com identidades grandes.</span><span class="sxs-lookup"><span data-stu-id="da2a2-191">`SessionStore` can be used to mitigate potential problems with large identities.</span></span> |
| [<span data-ttu-id="da2a2-192">SlidingExpiration</span><span class="sxs-lookup"><span data-stu-id="da2a2-192">SlidingExpiration</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-2.0) | <span data-ttu-id="da2a2-193">Um sinalizador que indica se um novo cookie com um tempo de expiração atualizados deve ser emitido dinamicamente.</span><span class="sxs-lookup"><span data-stu-id="da2a2-193">A flag indicating if a new cookie with an updated expiration time should be issued dynamically.</span></span> <span data-ttu-id="da2a2-194">Isso pode acontecer em qualquer solicitação em que o período de expiração do cookie atual mais de 50% expirou.</span><span class="sxs-lookup"><span data-stu-id="da2a2-194">This can happen on any request where the current cookie expiration period is more than 50% expired.</span></span> <span data-ttu-id="da2a2-195">A nova data de expiração é movida para frente para ser a data atual mais o `ExpireTimespan`.</span><span class="sxs-lookup"><span data-stu-id="da2a2-195">The new expiration date is moved forward to be the current date plus the `ExpireTimespan`.</span></span> <span data-ttu-id="da2a2-196">Um [tempo de expiração do cookie absoluto](xref:security/authentication/cookie#absolute-cookie-expiration) pode ser definida usando o `AuthenticationProperties` classe ao chamar `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="da2a2-196">An [absolute cookie expiration time](xref:security/authentication/cookie#absolute-cookie-expiration) can be set by using the `AuthenticationProperties` class when calling `SignInAsync`.</span></span> <span data-ttu-id="da2a2-197">Um tempo de expiração absoluta pode melhorar a segurança do seu aplicativo, limitando a quantidade de tempo que o cookie de autenticação é válido.</span><span class="sxs-lookup"><span data-stu-id="da2a2-197">An absolute expiration time can improve the security of your app by limiting the amount of time that the authentication cookie is valid.</span></span> <span data-ttu-id="da2a2-198">O valor padrão é `true`.</span><span class="sxs-lookup"><span data-stu-id="da2a2-198">The default value is `true`.</span></span> |
| [<span data-ttu-id="da2a2-199">TicketDataFormat</span><span class="sxs-lookup"><span data-stu-id="da2a2-199">TicketDataFormat</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.ticketdataformat?view=aspnetcore-2.0) | <span data-ttu-id="da2a2-200">O `TicketDataFormat` é usado para proteger e desproteger a identidade e outras propriedades que são armazenadas no valor do cookie.</span><span class="sxs-lookup"><span data-stu-id="da2a2-200">The `TicketDataFormat` is used to protect and unprotect the identity and other properties that are stored in the cookie value.</span></span> <span data-ttu-id="da2a2-201">Se não for fornecido, um `TicketDataFormat` é criado usando o [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="da2a2-201">If not provided, a `TicketDataFormat` is created using the [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0).</span></span> |
| [<span data-ttu-id="da2a2-202">Validar</span><span class="sxs-lookup"><span data-stu-id="da2a2-202">Validate</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.validate?view=aspnetcore-2.0) | <span data-ttu-id="da2a2-203">Método que verifica que as opções são válidas.</span><span class="sxs-lookup"><span data-stu-id="da2a2-203">Method that checks that the options are valid.</span></span> |

<span data-ttu-id="da2a2-204">Definir `CookieAuthenticationOptions` na configuração de serviço para autenticação no `ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="da2a2-204">Set `CookieAuthenticationOptions` in the service configuration for authentication in the `ConfigureServices` method:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="da2a2-205">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="da2a2-205">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="da2a2-206">ASP.NET Core 1. x usa cookie [middleware](xref:fundamentals/middleware) que serializa um objeto de usuário em um cookie criptografado.</span><span class="sxs-lookup"><span data-stu-id="da2a2-206">ASP.NET Core 1.x uses cookie [middleware](xref:fundamentals/middleware) that serializes a user principal into an encrypted cookie.</span></span> <span data-ttu-id="da2a2-207">Em solicitações subsequentes, o cookie é validado, e a entidade de segurança é recriada e atribuída ao `HttpContext.User` propriedade.</span><span class="sxs-lookup"><span data-stu-id="da2a2-207">On subsequent requests, the cookie is validated, and the principal is recreated and assigned to the `HttpContext.User` property.</span></span>

<span data-ttu-id="da2a2-208">Instalar o [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) pacote NuGet em seu projeto.</span><span class="sxs-lookup"><span data-stu-id="da2a2-208">Install the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) NuGet package in your project.</span></span> <span data-ttu-id="da2a2-209">Este pacote contém o middleware de cookie.</span><span class="sxs-lookup"><span data-stu-id="da2a2-209">This package contains the cookie middleware.</span></span>

<span data-ttu-id="da2a2-210">Use o `UseCookieAuthentication` método o `Configure` método no seu *Startup.cs* arquivo antes de `UseMvc` ou `UseMvcWithDefaultRoute`:</span><span class="sxs-lookup"><span data-stu-id="da2a2-210">Use the `UseCookieAuthentication` method in the `Configure` method in your *Startup.cs* file before `UseMvc` or `UseMvcWithDefaultRoute`:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions()
{
    AccessDeniedPath = "/Account/Forbidden/",
    AuthenticationScheme = CookieAuthenticationDefaults.AuthenticationScheme,
    AutomaticAuthenticate = true,
    AutomaticChallenge = true,
    LoginPath = "/Account/Unauthorized/"
});
```

<span data-ttu-id="da2a2-211">**Opções de CookieAuthenticationOptions**</span><span class="sxs-lookup"><span data-stu-id="da2a2-211">**CookieAuthenticationOptions Options**</span></span>

<span data-ttu-id="da2a2-212">O [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions?view=aspnetcore-1.1) classe é usada para configurar as opções de provedor de autenticação.</span><span class="sxs-lookup"><span data-stu-id="da2a2-212">The [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions?view=aspnetcore-1.1) class is used to configure the authentication provider options.</span></span>

| <span data-ttu-id="da2a2-213">Opção</span><span class="sxs-lookup"><span data-stu-id="da2a2-213">Option</span></span> | <span data-ttu-id="da2a2-214">Descrição</span><span class="sxs-lookup"><span data-stu-id="da2a2-214">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="da2a2-215">AuthenticationScheme</span><span class="sxs-lookup"><span data-stu-id="da2a2-215">AuthenticationScheme</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.authenticationscheme?view=aspnetcore-1.1) | <span data-ttu-id="da2a2-216">Define o esquema de autenticação.</span><span class="sxs-lookup"><span data-stu-id="da2a2-216">Sets the authentication scheme.</span></span> <span data-ttu-id="da2a2-217">`AuthenticationScheme`é útil quando há várias instâncias de autenticação e você deseja [autorizar com um esquema específico](xref:security/authorization/limitingidentitybyscheme).</span><span class="sxs-lookup"><span data-stu-id="da2a2-217">`AuthenticationScheme` is useful when there are multiple instances of authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="da2a2-218">Definindo o `AuthenticationScheme` para `CookieAuthenticationDefaults.AuthenticationScheme` fornece um valor de "Cookies" para o esquema.</span><span class="sxs-lookup"><span data-stu-id="da2a2-218">Setting the `AuthenticationScheme` to `CookieAuthenticationDefaults.AuthenticationScheme` provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="da2a2-219">Você pode fornecer qualquer valor de cadeia de caracteres que diferencia o esquema.</span><span class="sxs-lookup"><span data-stu-id="da2a2-219">You can supply any string value that distinguishes the scheme.</span></span> |
| [<span data-ttu-id="da2a2-220">AutomaticAuthenticate</span><span class="sxs-lookup"><span data-stu-id="da2a2-220">AutomaticAuthenticate</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticauthenticate?view=aspnetcore-1.1) | <span data-ttu-id="da2a2-221">Define um valor para indicar que a autenticação de cookie deve executar em cada solicitação e tente validar e reconstrua qualquer entidade de segurança serializada criado por ele.</span><span class="sxs-lookup"><span data-stu-id="da2a2-221">Sets a value to indicate that the cookie authentication should run on every request and attempt to validate and reconstruct any serialized principal it created.</span></span> |
| [<span data-ttu-id="da2a2-222">AutomaticChallenge</span><span class="sxs-lookup"><span data-stu-id="da2a2-222">AutomaticChallenge</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticchallenge?view=aspnetcore-1.1) | <span data-ttu-id="da2a2-223">Se for true, o middleware de autenticação manipula desafios automática.</span><span class="sxs-lookup"><span data-stu-id="da2a2-223">If true, the authentication middleware handles automatic challenges.</span></span> <span data-ttu-id="da2a2-224">Se false, o middleware de autenticação altera somente respostas quando explicitamente indicado pelo `AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="da2a2-224">If false, the authentication middleware only alters responses when explicitly indicated by the `AuthenticationScheme`.</span></span> |
| [<span data-ttu-id="da2a2-225">ClaimsIssuer</span><span class="sxs-lookup"><span data-stu-id="da2a2-225">ClaimsIssuer</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.claimsissuer?view=aspnetcore-1.1) | <span data-ttu-id="da2a2-226">O emissor a ser usado para o [emissor](/dotnet/api/system.security.claims.claim.issuer) propriedade em quaisquer declarações criado pelo middleware de autenticação de cookie.</span><span class="sxs-lookup"><span data-stu-id="da2a2-226">The issuer to use for the [Issuer](/dotnet/api/system.security.claims.claim.issuer) property on any claims created by the cookie authentication middleware.</span></span> |
| [<span data-ttu-id="da2a2-227">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="da2a2-227">CookieDomain</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiedomain?view=aspnetcore-1.1) | <span data-ttu-id="da2a2-228">O nome de domínio em que o cookie é atendido.</span><span class="sxs-lookup"><span data-stu-id="da2a2-228">The domain name where the cookie is served.</span></span> <span data-ttu-id="da2a2-229">Por padrão, esse é o nome do host da solicitação.</span><span class="sxs-lookup"><span data-stu-id="da2a2-229">By default, this is the host name of the request.</span></span> <span data-ttu-id="da2a2-230">O navegador serve apenas o cookie para um nome de host correspondente.</span><span class="sxs-lookup"><span data-stu-id="da2a2-230">The browser only serves the cookie to a matching host name.</span></span> <span data-ttu-id="da2a2-231">Talvez você queira ajustar isso para que os cookies disponíveis para qualquer host no seu domínio.</span><span class="sxs-lookup"><span data-stu-id="da2a2-231">You may wish to adjust this to have cookies available to any host in your domain.</span></span> <span data-ttu-id="da2a2-232">Por exemplo, definir o domínio do cookie `.contoso.com` disponibiliza para `contoso.com`, `www.contoso.com`, e `staging.www.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="da2a2-232">For example, setting the cookie domain to `.contoso.com` makes it available to `contoso.com`, `www.contoso.com`, and `staging.www.contoso.com`.</span></span> |
| [<span data-ttu-id="da2a2-233">CookieHttpOnly</span><span class="sxs-lookup"><span data-stu-id="da2a2-233">CookieHttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiehttponly?view=aspnetcore-1.1) | <span data-ttu-id="da2a2-234">Um sinalizador que indica se o cookie deve ser acessível somente para servidores.</span><span class="sxs-lookup"><span data-stu-id="da2a2-234">A flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="da2a2-235">A alteração desse valor para `false` permite que scripts do lado do cliente para acessar o cookie e pode abrir o aplicativo para roubo de cookie deve ter seu aplicativo um [script entre sites (XSS)](xref:security/cross-site-scripting) vulnerabilidade.</span><span class="sxs-lookup"><span data-stu-id="da2a2-235">Changing this value to `false` permits client-side scripts to access the cookie and may open your app to cookie theft should your app have a [Cross-site scripting (XSS)](xref:security/cross-site-scripting) vulnerability.</span></span> <span data-ttu-id="da2a2-236">O valor padrão é `true`.</span><span class="sxs-lookup"><span data-stu-id="da2a2-236">The default value is `true`.</span></span> |
| [<span data-ttu-id="da2a2-237">CookiePath</span><span class="sxs-lookup"><span data-stu-id="da2a2-237">CookiePath</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiepath?view=aspnetcore-1.1) | <span data-ttu-id="da2a2-238">Usado para isolar os aplicativos em execução no mesmo nome de host.</span><span class="sxs-lookup"><span data-stu-id="da2a2-238">Used to isolate apps running on the same host name.</span></span> <span data-ttu-id="da2a2-239">Se você tiver um aplicativo em execução no `/app1` e deseja restringir os cookies para esse aplicativo, defina o `CookiePath` propriedade `/app1`.</span><span class="sxs-lookup"><span data-stu-id="da2a2-239">If you have an app running at `/app1` and want to restrict cookies to that app, set the `CookiePath` property to `/app1`.</span></span> <span data-ttu-id="da2a2-240">Por isso, o cookie só está disponível em solicitações para `/app1` e qualquer aplicativo abaixo dele.</span><span class="sxs-lookup"><span data-stu-id="da2a2-240">By doing so, the cookie is only available on requests to `/app1` and any app underneath it.</span></span> |
| [<span data-ttu-id="da2a2-241">CookieSecure</span><span class="sxs-lookup"><span data-stu-id="da2a2-241">CookieSecure</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiesecure?view=aspnetcore-1.1) | <span data-ttu-id="da2a2-242">Um sinalizador que indica se o cookie criado deve ser limitado para HTTPS (`CookieSecurePolicy.Always`), HTTP ou HTTPS (`CookieSecurePolicy.None`), ou o mesmo protocolo que a solicitação (`CookieSecurePolicy.SameAsRequest`).</span><span class="sxs-lookup"><span data-stu-id="da2a2-242">A flag indicating if the cookie created should be limited to HTTPS (`CookieSecurePolicy.Always`), HTTP or HTTPS (`CookieSecurePolicy.None`), or the same protocol as the request (`CookieSecurePolicy.SameAsRequest`).</span></span> <span data-ttu-id="da2a2-243">O valor padrão é `CookieSecurePolicy.SameAsRequest`.</span><span class="sxs-lookup"><span data-stu-id="da2a2-243">The default value is `CookieSecurePolicy.SameAsRequest`.</span></span> |
| [<span data-ttu-id="da2a2-244">Descrição</span><span class="sxs-lookup"><span data-stu-id="da2a2-244">Description</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.description?view=aspnetcore-1.1) | <span data-ttu-id="da2a2-245">Informações adicionais sobre o tipo de autenticação que é disponibilizado para o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="da2a2-245">Additional information about the authentication type which is made available to the app.</span></span> |
| [<span data-ttu-id="da2a2-246">ExpireTimeSpan</span><span class="sxs-lookup"><span data-stu-id="da2a2-246">ExpireTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.expiretimespan?view=aspnetcore-1.1) | <span data-ttu-id="da2a2-247">O `TimeSpan` depois que o tíquete de autenticação expira.</span><span class="sxs-lookup"><span data-stu-id="da2a2-247">The `TimeSpan` after which the authentication ticket expires.</span></span> <span data-ttu-id="da2a2-248">Ele é adicionado para a hora atual para criar o tempo de expiração do tíquete de.</span><span class="sxs-lookup"><span data-stu-id="da2a2-248">It's added to the current time to create the expiration time for the ticket.</span></span> <span data-ttu-id="da2a2-249">Para usar `ExpireTimeSpan`, você deve definir `IsPersistent` para `true` no `AuthenticationProperties` passado para `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="da2a2-249">To use `ExpireTimeSpan`, you must set `IsPersistent` to `true` in the `AuthenticationProperties` passed to `SignInAsync`.</span></span> <span data-ttu-id="da2a2-250">O valor padrão é 14 dias.</span><span class="sxs-lookup"><span data-stu-id="da2a2-250">The default value is 14 days.</span></span> |
| [<span data-ttu-id="da2a2-251">SlidingExpiration</span><span class="sxs-lookup"><span data-stu-id="da2a2-251">SlidingExpiration</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-1.1) | <span data-ttu-id="da2a2-252">Um sinalizador que indica se a data de expiração do cookie redefine quando houver mais de metade do `ExpireTimeSpan` do intervalo.</span><span class="sxs-lookup"><span data-stu-id="da2a2-252">A flag indicating whether the cookie expiration date resets when more than half of the `ExpireTimeSpan` interval has passed.</span></span> <span data-ttu-id="da2a2-253">A nova hora de exipiration é movida para frente para ser a data atual mais o `ExpireTimespan`.</span><span class="sxs-lookup"><span data-stu-id="da2a2-253">The new exipiration time is moved forward to be the current date plus the `ExpireTimespan`.</span></span> <span data-ttu-id="da2a2-254">Um [tempo de expiração do cookie absoluto](xref:security/authentication/cookie#absolute-cookie-expiration) pode ser definida usando o `AuthenticationProperties` classe ao chamar `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="da2a2-254">An [absolute cookie expiration time](xref:security/authentication/cookie#absolute-cookie-expiration) can be set by using the `AuthenticationProperties` class when calling `SignInAsync`.</span></span> <span data-ttu-id="da2a2-255">Um tempo de expiração absoluta pode melhorar a segurança do seu aplicativo, limitando a quantidade de tempo que o cookie de autenticação é válido.</span><span class="sxs-lookup"><span data-stu-id="da2a2-255">An absolute expiration time can improve the security of your app by limiting the amount of time that the authentication cookie is valid.</span></span> <span data-ttu-id="da2a2-256">O valor padrão é `true`.</span><span class="sxs-lookup"><span data-stu-id="da2a2-256">The default value is `true`.</span></span> |

<span data-ttu-id="da2a2-257">Definir `CookieAuthenticationOptions` para o Middleware de autenticação de Cookie no `Configure` método:</span><span class="sxs-lookup"><span data-stu-id="da2a2-257">Set `CookieAuthenticationOptions` for the Cookie Authentication Middleware in the `Configure` method:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    ...
});
```

---

## <a name="cookie-policy-middleware"></a><span data-ttu-id="da2a2-258">Middleware de política de cookie</span><span class="sxs-lookup"><span data-stu-id="da2a2-258">Cookie Policy Middleware</span></span>

<span data-ttu-id="da2a2-259">[Middleware do cookie política](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware) habilita recursos de política de cookie em um aplicativo.</span><span class="sxs-lookup"><span data-stu-id="da2a2-259">[Cookie Policy Middleware](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware) enables cookie policy capabilities in an app.</span></span> <span data-ttu-id="da2a2-260">Adicionar o middleware para o pipeline de processamento de aplicativos é a ordem e minúsculas; ela afeta apenas os componentes registrados depois no pipeline.</span><span class="sxs-lookup"><span data-stu-id="da2a2-260">Adding the middleware to the app processing pipeline is order sensitive; it only affects components registered after it in the pipeline.</span></span>

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

 <span data-ttu-id="da2a2-261">O [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) fornecido para o Middleware do Cookie política permitem que você controle características globais do processamento de cookie e gancho em manipuladores de processamento do cookie quando os cookies são anexados ou excluídos.</span><span class="sxs-lookup"><span data-stu-id="da2a2-261">The [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) provided to the Cookie Policy Middleware allow you to control global characteristics of cookie processing and hook into cookie processing handlers when cookies are appended or deleted.</span></span>

| <span data-ttu-id="da2a2-262">Propriedade</span><span class="sxs-lookup"><span data-stu-id="da2a2-262">Property</span></span> | <span data-ttu-id="da2a2-263">Descrição</span><span class="sxs-lookup"><span data-stu-id="da2a2-263">Description</span></span> |
| -------- | ----------- |
| [<span data-ttu-id="da2a2-264">HttpOnly</span><span class="sxs-lookup"><span data-stu-id="da2a2-264">HttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.httponly) | <span data-ttu-id="da2a2-265">Afeta se os cookies devem ser HttpOnly, que é um sinalizador que indica se o cookie deve ser acessível somente para servidores.</span><span class="sxs-lookup"><span data-stu-id="da2a2-265">Affects whether cookies must be HttpOnly, which is a flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="da2a2-266">O valor padrão é `HttpOnlyPolicy.None`.</span><span class="sxs-lookup"><span data-stu-id="da2a2-266">The default value is `HttpOnlyPolicy.None`.</span></span> |
| [<span data-ttu-id="da2a2-267">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="da2a2-267">MinimumSameSitePolicy</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.minimumsamesitepolicy) | <span data-ttu-id="da2a2-268">Afeta o atributo de mesmo site do cookie (veja abaixo).</span><span class="sxs-lookup"><span data-stu-id="da2a2-268">Affects the cookie's same-site attribute (see below).</span></span> <span data-ttu-id="da2a2-269">O valor padrão é `SameSiteMode.Lax`.</span><span class="sxs-lookup"><span data-stu-id="da2a2-269">The default value is `SameSiteMode.Lax`.</span></span> <span data-ttu-id="da2a2-270">Essa opção está disponível para o ASP.NET Core 2.0 +.</span><span class="sxs-lookup"><span data-stu-id="da2a2-270">This option is available for ASP.NET Core 2.0+.</span></span> |
| [<span data-ttu-id="da2a2-271">OnAppendCookie</span><span class="sxs-lookup"><span data-stu-id="da2a2-271">OnAppendCookie</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.onappendcookie) | <span data-ttu-id="da2a2-272">Chamado quando um cookie é anexado.</span><span class="sxs-lookup"><span data-stu-id="da2a2-272">Called when a cookie is appended.</span></span> |
| [<span data-ttu-id="da2a2-273">OnDeleteCookie</span><span class="sxs-lookup"><span data-stu-id="da2a2-273">OnDeleteCookie</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.ondeletecookie) | <span data-ttu-id="da2a2-274">Chamado quando um cookie é excluído.</span><span class="sxs-lookup"><span data-stu-id="da2a2-274">Called when a cookie is deleted.</span></span> |
| [<span data-ttu-id="da2a2-275">Proteger</span><span class="sxs-lookup"><span data-stu-id="da2a2-275">Secure</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.secure) | <span data-ttu-id="da2a2-276">Afeta se os cookies devem ser seguro.</span><span class="sxs-lookup"><span data-stu-id="da2a2-276">Affects whether cookies must be Secure.</span></span> <span data-ttu-id="da2a2-277">O valor padrão é `CookieSecurePolicy.None`.</span><span class="sxs-lookup"><span data-stu-id="da2a2-277">The default value is `CookieSecurePolicy.None`.</span></span> |

<span data-ttu-id="da2a2-278">**MinimumSameSitePolicy** (ASP.NET Core 2.0 + apenas)</span><span class="sxs-lookup"><span data-stu-id="da2a2-278">**MinimumSameSitePolicy** (ASP.NET Core 2.0+ only)</span></span>

<span data-ttu-id="da2a2-279">O padrão `MinimumSameSitePolicy` valor é `SameSiteMode.Lax` para permitir a autenticação OAuth2.</span><span class="sxs-lookup"><span data-stu-id="da2a2-279">The default `MinimumSameSitePolicy` value is `SameSiteMode.Lax` to permit OAuth2 authentication.</span></span> <span data-ttu-id="da2a2-280">Estritamente impor uma política do mesmo site do `SameSiteMode.Strict`, defina o `MinimumSameSitePolicy`.</span><span class="sxs-lookup"><span data-stu-id="da2a2-280">To strictly enforce a same-site policy of `SameSiteMode.Strict`, set the `MinimumSameSitePolicy`.</span></span> <span data-ttu-id="da2a2-281">Embora essa configuração quebras OAuth2 e outros esquemas de autenticação entre origens, ele eleva o nível de segurança do cookie para outros tipos de aplicativos que não dependem de processamento de solicitação entre origens.</span><span class="sxs-lookup"><span data-stu-id="da2a2-281">Although this setting breaks OAuth2 and other cross-origin authentication schemes, it elevates the level of cookie security for other types of apps that don't rely on cross-origin request processing.</span></span>

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

<span data-ttu-id="da2a2-282">A configuração de Middleware de política de Cookie para `MinimumSameSitePolicy` pode afetar sua configuração de `Cookie.SameSite` na `CookieAuthenticationOptions` configurações de acordo com a matriz abaixo.</span><span class="sxs-lookup"><span data-stu-id="da2a2-282">The Cookie Policy Middleware setting for `MinimumSameSitePolicy` can affect your setting of `Cookie.SameSite` in `CookieAuthenticationOptions` settings according to the matrix below.</span></span>

| <span data-ttu-id="da2a2-283">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="da2a2-283">MinimumSameSitePolicy</span></span> | <span data-ttu-id="da2a2-284">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="da2a2-284">Cookie.SameSite</span></span> | <span data-ttu-id="da2a2-285">Configuração de Cookie.SameSite resultante</span><span class="sxs-lookup"><span data-stu-id="da2a2-285">Resultant Cookie.SameSite setting</span></span> |
| --------------------- | --------------- | --------------------------------- |
| <span data-ttu-id="da2a2-286">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="da2a2-286">SameSiteMode.None</span></span>     | <span data-ttu-id="da2a2-287">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="da2a2-287">SameSiteMode.None</span></span><br><span data-ttu-id="da2a2-288">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="da2a2-288">SameSiteMode.Lax</span></span><br><span data-ttu-id="da2a2-289">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="da2a2-289">SameSiteMode.Strict</span></span> | <span data-ttu-id="da2a2-290">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="da2a2-290">SameSiteMode.None</span></span><br><span data-ttu-id="da2a2-291">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="da2a2-291">SameSiteMode.Lax</span></span><br><span data-ttu-id="da2a2-292">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="da2a2-292">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="da2a2-293">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="da2a2-293">SameSiteMode.Lax</span></span>      | <span data-ttu-id="da2a2-294">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="da2a2-294">SameSiteMode.None</span></span><br><span data-ttu-id="da2a2-295">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="da2a2-295">SameSiteMode.Lax</span></span><br><span data-ttu-id="da2a2-296">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="da2a2-296">SameSiteMode.Strict</span></span> | <span data-ttu-id="da2a2-297">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="da2a2-297">SameSiteMode.Lax</span></span><br><span data-ttu-id="da2a2-298">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="da2a2-298">SameSiteMode.Lax</span></span><br><span data-ttu-id="da2a2-299">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="da2a2-299">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="da2a2-300">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="da2a2-300">SameSiteMode.Strict</span></span>   | <span data-ttu-id="da2a2-301">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="da2a2-301">SameSiteMode.None</span></span><br><span data-ttu-id="da2a2-302">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="da2a2-302">SameSiteMode.Lax</span></span><br><span data-ttu-id="da2a2-303">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="da2a2-303">SameSiteMode.Strict</span></span> | <span data-ttu-id="da2a2-304">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="da2a2-304">SameSiteMode.Strict</span></span><br><span data-ttu-id="da2a2-305">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="da2a2-305">SameSiteMode.Strict</span></span><br><span data-ttu-id="da2a2-306">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="da2a2-306">SameSiteMode.Strict</span></span> |

## <a name="creating-an-authentication-cookie"></a><span data-ttu-id="da2a2-307">Criando um cookie de autenticação</span><span class="sxs-lookup"><span data-stu-id="da2a2-307">Creating an authentication cookie</span></span>

<span data-ttu-id="da2a2-308">Para criar um cookie contendo informações de usuário, você precisa construir uma [ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal).</span><span class="sxs-lookup"><span data-stu-id="da2a2-308">To create a cookie holding user information, you must construct a [ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal).</span></span> <span data-ttu-id="da2a2-309">As informações do usuário são serializadas e armazenadas no cookie.</span><span class="sxs-lookup"><span data-stu-id="da2a2-309">The user information is serialized and stored in the cookie.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="da2a2-310">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="da2a2-310">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="da2a2-311">Chamar [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0) para assinar o usuário:</span><span class="sxs-lookup"><span data-stu-id="da2a2-311">Call [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0) to sign in the user:</span></span>

```csharp
await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme, 
    new ClaimsPrincipal(claimsIdentity));
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="da2a2-312">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="da2a2-312">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="da2a2-313">Chamar [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1) para assinar o usuário:</span><span class="sxs-lookup"><span data-stu-id="da2a2-313">Call [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1) to sign in the user:</span></span>

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity));
```

---

<span data-ttu-id="da2a2-314">`SignInAsync`cria um cookie criptografado e o adiciona à resposta atual.</span><span class="sxs-lookup"><span data-stu-id="da2a2-314">`SignInAsync` creates an encrypted cookie and adds it to the current response.</span></span> <span data-ttu-id="da2a2-315">Se você não especificar um `AuthenticationScheme`, o esquema padrão é usado.</span><span class="sxs-lookup"><span data-stu-id="da2a2-315">If you don't specify an `AuthenticationScheme`, the default scheme is used.</span></span>

<span data-ttu-id="da2a2-316">Nos bastidores, a criptografia é ASP.NET Core [proteção de dados](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) sistema.</span><span class="sxs-lookup"><span data-stu-id="da2a2-316">Under the covers, the encryption used is ASP.NET Core's [Data Protection](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) system.</span></span> <span data-ttu-id="da2a2-317">Se você estiver hospedando o aplicativo em vários computadores, balanceamento de carga em aplicativos ou usando uma web farm, você deve [configurar a proteção de dados](xref:security/data-protection/configuration/overview) para usar o mesmo anel de chave e o identificador do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="da2a2-317">If you're hosting app on multiple machines, load balancing across apps, or using a web farm, then you must [configure data protection](xref:security/data-protection/configuration/overview) to use the same key ring and app identifier.</span></span>

## <a name="signing-out"></a><span data-ttu-id="da2a2-318">Sair</span><span class="sxs-lookup"><span data-stu-id="da2a2-318">Signing out</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="da2a2-319">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="da2a2-319">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="da2a2-320">Para desconectar o usuário atual e exclua seus cookies, chame [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):</span><span class="sxs-lookup"><span data-stu-id="da2a2-320">To sign out the current user and delete their cookie, call [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):</span></span>

```csharp
await HttpContext.SignOutAsync(
    CookieAuthenticationDefaults.AuthenticationScheme);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="da2a2-321">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="da2a2-321">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="da2a2-322">Para desconectar o usuário atual e exclua seus cookies, chame [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):</span><span class="sxs-lookup"><span data-stu-id="da2a2-322">To sign out the current user and delete their cookie, call [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):</span></span>

```csharp
await HttpContext.Authentication.SignOutAsync(
    CookieAuthenticationDefaults.AuthenticationScheme);
```

---

<span data-ttu-id="da2a2-323">Se você não estiver usando `CookieAuthenticationDefaults.AuthenticationScheme` (ou "Cookies") como o esquema (por exemplo, "ContosoCookie"), forneça o esquema usado ao configurar o provedor de autenticação.</span><span class="sxs-lookup"><span data-stu-id="da2a2-323">If you aren't using `CookieAuthenticationDefaults.AuthenticationScheme` (or "Cookies") as the scheme (for example, "ContosoCookie"), supply the scheme you used when configuring the authentication provider.</span></span> <span data-ttu-id="da2a2-324">Caso contrário, o esquema padrão será usado.</span><span class="sxs-lookup"><span data-stu-id="da2a2-324">Otherwise, the default scheme is used.</span></span>

## <a name="reacting-to-back-end-changes"></a><span data-ttu-id="da2a2-325">Reagir a alterações de back-end</span><span class="sxs-lookup"><span data-stu-id="da2a2-325">Reacting to back-end changes</span></span>

<span data-ttu-id="da2a2-326">Quando um cookie é criado, ele se torna a única fonte de identidade.</span><span class="sxs-lookup"><span data-stu-id="da2a2-326">Once a cookie is created, it becomes the single source of identity.</span></span> <span data-ttu-id="da2a2-327">Mesmo se você desabilitar um usuário nos seus sistemas de back-end, o sistema de autenticação de cookie não tem conhecimento sobre isso e um usuário permanece conectado como o cookie é válido.</span><span class="sxs-lookup"><span data-stu-id="da2a2-327">Even if you disable a user in your back-end systems, the cookie authentication system has no knowledge of this, and a user stays logged in as long as their cookie is valid.</span></span>

<span data-ttu-id="da2a2-328">O [ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal) evento no ASP.NET Core 2. x ou [ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1) método no núcleo do ASP.NET 1. x pode ser usado para interceptar e ignorar a validação da identidade do cookie.</span><span class="sxs-lookup"><span data-stu-id="da2a2-328">The [ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal) event in ASP.NET Core 2.x or the [ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1) method in ASP.NET Core 1.x can be used to intercept and override validation of the cookie identity.</span></span> <span data-ttu-id="da2a2-329">Isso reduz o risco dos usuários revogados acessando o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="da2a2-329">This approach mitigates the risk of revoked users accessing the app.</span></span>

<span data-ttu-id="da2a2-330">Uma abordagem para validação de cookie baseia-se em manter o controle de quando o banco de dados do usuário foi alterado.</span><span class="sxs-lookup"><span data-stu-id="da2a2-330">One approach to cookie validation is based on keeping track of when the user database has been changed.</span></span> <span data-ttu-id="da2a2-331">Se o banco de dados não foi alterado desde que o cookie do usuário foi emitido, não é necessário para autenticar o usuário novamente se o cookie ainda é válido.</span><span class="sxs-lookup"><span data-stu-id="da2a2-331">If the database hasn't been changed since the user's cookie was issued, there is no need to re-authenticate the user if their cookie is still valid.</span></span> <span data-ttu-id="da2a2-332">Para implementar este cenário, o banco de dados, que é implementado no `IUserRepository` para este exemplo armazena um `LastChanged` valor.</span><span class="sxs-lookup"><span data-stu-id="da2a2-332">To implement this scenario, the database, which is implemented in `IUserRepository` for this example, stores a `LastChanged` value.</span></span> <span data-ttu-id="da2a2-333">Quando nenhum usuário é atualizado no banco de dados, o `LastChanged` valor é definido para a hora atual.</span><span class="sxs-lookup"><span data-stu-id="da2a2-333">When any user is updated in the database, the `LastChanged` value is set to the current time.</span></span>

<span data-ttu-id="da2a2-334">Para invalidar um cookie quando as alterações do banco de dados com base no `LastChanged` valor, criar o cookie com um `LastChanged` declaração contendo atual `LastChanged` valor do banco de dados:</span><span class="sxs-lookup"><span data-stu-id="da2a2-334">In order to invalidate a cookie when the database changes based on the `LastChanged` value, create the cookie with a `LastChanged` claim containing the current `LastChanged` value from the database:</span></span>

```csharp
var claims = new List<Claim>
{
    new Claim(ClaimTypes.Name, user.Email),
    new Claim("LastChanged", {Database Value})
};

var claimsIdentity = new ClaimsIdentity(
    claims, 
    CookieAuthenticationDefaults.AuthenticationScheme);

await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme, 
    new ClaimsPrincipal(claimsIdentity));
```

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="da2a2-335">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="da2a2-335">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="da2a2-336">Para implementar uma substituição para o `ValidatePrincipal` evento, a gravação de um método com a seguinte assinatura em uma classe que deriva de [CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):</span><span class="sxs-lookup"><span data-stu-id="da2a2-336">To implement an override for the `ValidatePrincipal` event, write a method with the following signature in a class that you derive from [CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):</span></span>

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

<span data-ttu-id="da2a2-337">Um exemplo é semelhante ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="da2a2-337">An example looks like the following:</span></span>

```csharp
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Authentication;
using Microsoft.AspNetCore.Authentication.Cookies;

public class CustomCookieAuthenticationEvents : CookieAuthenticationEvents
{
    private readonly IUserRepository _userRepository;

    public CustomCookieAuthenticationEvents(IUserRepository userRepository)
    {
        // Get the database from registered DI services.
        _userRepository = userRepository;
    }

    public override async Task ValidatePrincipal(CookieValidatePrincipalContext context)
    {
        var userPrincipal = context.Principal;

        // Look for the LastChanged claim.
        var lastChanged = (from c in userPrincipal.Claims
                           where c.Type == "LastChanged"
                           select c.Value).FirstOrDefault();

        if (string.IsNullOrEmpty(lastChanged) ||
            !_userRepository.ValidateLastChanged(lastChanged))
        {
            context.RejectPrincipal();

            await context.HttpContext.SignOutAsync(
                CookieAuthenticationDefaults.AuthenticationScheme);
        }
    }
}
```

<span data-ttu-id="da2a2-338">Registrar a instância de eventos durante o registro do serviço de cookie no `ConfigureServices` método.</span><span class="sxs-lookup"><span data-stu-id="da2a2-338">Register the events instance during cookie service registration in the `ConfigureServices` method.</span></span> <span data-ttu-id="da2a2-339">Forneça um registro de serviço com escopo definido para seu `CustomCookieAuthenticationEvents` classe:</span><span class="sxs-lookup"><span data-stu-id="da2a2-339">Provide a scoped service registration for your `CustomCookieAuthenticationEvents` class:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="da2a2-340">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="da2a2-340">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="da2a2-341">Para implementar uma substituição para o `ValidateAsync` evento, a gravação de um método com a seguinte assinatura:</span><span class="sxs-lookup"><span data-stu-id="da2a2-341">To implement an override for the `ValidateAsync` event, write a method with the following signature:</span></span>

```csharp
ValidateAsync(CookieValidatePrincipalContext)
```

<span data-ttu-id="da2a2-342">A identidade do ASP.NET Core implementa essa verificação como parte de seu [SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync).</span><span class="sxs-lookup"><span data-stu-id="da2a2-342">ASP.NET Core Identity implements this check as part of its [SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync).</span></span> <span data-ttu-id="da2a2-343">Um exemplo é semelhante ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="da2a2-343">An example looks like the following:</span></span>

```csharp
public static class LastChangedValidator
{
    public static async Task ValidateAsync(CookieValidatePrincipalContext context)
    {
        // Pull database from registered DI services.
        var userRepository = 
            context.HttpContext.RequestServices
                .GetRequiredService<IUserRepository>();
        var userPrincipal = context.Principal;

        // Look for the last changed claim.
        var lastChanged = (from c in userPrincipal.Claims
                           where c.Type == "LastChanged"
                           select c.Value).FirstOrDefault();

        if (string.IsNullOrEmpty(lastChanged) ||
            !userRepository.ValidateLastChanged(lastChanged))
        {
            context.RejectPrincipal();

            await context.HttpContext.SignOutAsync(
                CookieAuthenticationDefaults.AuthenticationScheme);
        }
    }
}
```

<span data-ttu-id="da2a2-344">Registrar o evento durante a configuração de autenticação de cookie no `Configure` método:</span><span class="sxs-lookup"><span data-stu-id="da2a2-344">Register the event during cookie authentication configuration in the `Configure` method:</span></span>

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

<span data-ttu-id="da2a2-345">Considere uma situação em que o nome do usuário é atualizado &mdash; uma decisão que não afeta a segurança de qualquer forma.</span><span class="sxs-lookup"><span data-stu-id="da2a2-345">Consider a situation in which the user's name is updated &mdash; a decision that doesn't affect security in any way.</span></span> <span data-ttu-id="da2a2-346">Se você quiser atualizar não destrutivo a entidade de usuário, chame `context.ReplacePrincipal` e defina o `context.ShouldRenew` propriedade `true`.</span><span class="sxs-lookup"><span data-stu-id="da2a2-346">If you want to non-destructively update the user principal, call `context.ReplacePrincipal` and set the `context.ShouldRenew` property to `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="da2a2-347">A abordagem descrita aqui é acionada em cada solicitação.</span><span class="sxs-lookup"><span data-stu-id="da2a2-347">The approach described here is triggered on every request.</span></span> <span data-ttu-id="da2a2-348">Isso pode resultar em uma penalidade de desempenho para o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="da2a2-348">This can result in a large performance penalty for the app.</span></span>

## <a name="persistent-cookies"></a><span data-ttu-id="da2a2-349">Cookies persistentes</span><span class="sxs-lookup"><span data-stu-id="da2a2-349">Persistent cookies</span></span>

<span data-ttu-id="da2a2-350">Talvez você queira que o cookie para persistir nas sessões do navegador.</span><span class="sxs-lookup"><span data-stu-id="da2a2-350">You may want the cookie to persist across browser sessions.</span></span> <span data-ttu-id="da2a2-351">Essa persistência deve ser habilitada apenas com a permissão explícita do usuário com uma "Lembrar-Me" caixa de seleção no logon ou um mecanismo semelhante.</span><span class="sxs-lookup"><span data-stu-id="da2a2-351">This persistence should only be enabled with explicit user consent with a "Remember Me" checkbox on login or a similar mechanism.</span></span> 

<span data-ttu-id="da2a2-352">O trecho de código a seguir cria uma identidade e o cookie correspondente que sobrevive a fechamentos de navegador.</span><span class="sxs-lookup"><span data-stu-id="da2a2-352">The following code snippet creates an identity and corresponding cookie that survives through browser closures.</span></span> <span data-ttu-id="da2a2-353">As configurações de expiração deslizante configuradas anteriormente são respeitadas.</span><span class="sxs-lookup"><span data-stu-id="da2a2-353">Any sliding expiration settings previously configured are honored.</span></span> <span data-ttu-id="da2a2-354">Se o cookie expirar enquanto o navegador for fechado, o navegador limpa o cookie depois que ele seja reiniciado.</span><span class="sxs-lookup"><span data-stu-id="da2a2-354">If the cookie expires while the browser is closed, the browser clears the cookie once it's restarted.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="da2a2-355">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="da2a2-355">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

<span data-ttu-id="da2a2-356">O [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties?view=aspnetcore-2.0) classe reside no `Microsoft.AspNetCore.Authentication` namespace.</span><span class="sxs-lookup"><span data-stu-id="da2a2-356">The [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties?view=aspnetcore-2.0) class resides in the `Microsoft.AspNetCore.Authentication` namespace.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="da2a2-357">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="da2a2-357">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

<span data-ttu-id="da2a2-358">O [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.http.authentication.authenticationproperties?view=aspnetcore-1.1) classe reside no `Microsoft.AspNetCore.Http.Authentication` namespace.</span><span class="sxs-lookup"><span data-stu-id="da2a2-358">The [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.http.authentication.authenticationproperties?view=aspnetcore-1.1) class resides in the `Microsoft.AspNetCore.Http.Authentication` namespace.</span></span>

---

## <a name="absolute-cookie-expiration"></a><span data-ttu-id="da2a2-359">Expiração do cookie absoluto</span><span class="sxs-lookup"><span data-stu-id="da2a2-359">Absolute cookie expiration</span></span>

<span data-ttu-id="da2a2-360">Você pode definir um tempo de expiração absoluta com `ExpiresUtc`.</span><span class="sxs-lookup"><span data-stu-id="da2a2-360">You can set an absolute expiration time with `ExpiresUtc`.</span></span> <span data-ttu-id="da2a2-361">Você também deve definir `IsPersistent`; caso contrário, `ExpiresUtc` é ignorada e um cookie de sessão único é criado.</span><span class="sxs-lookup"><span data-stu-id="da2a2-361">You must also set `IsPersistent`; otherwise, `ExpiresUtc` is ignored and a single-session cookie is created.</span></span> <span data-ttu-id="da2a2-362">Quando `ExpiresUtc` é definido em `SignInAsync`, ela substitui o valor da `ExpireTimeSpan` opção de `CookieAuthenticationOptions`, se definido.</span><span class="sxs-lookup"><span data-stu-id="da2a2-362">When `ExpiresUtc` is set on `SignInAsync`, it overrides the value of the `ExpireTimeSpan` option of `CookieAuthenticationOptions`, if set.</span></span>

<span data-ttu-id="da2a2-363">O trecho de código a seguir cria uma identidade e o cookie correspondente que tem duração de 20 minutos.</span><span class="sxs-lookup"><span data-stu-id="da2a2-363">The following code snippet creates an identity and corresponding cookie that lasts for 20 minutes.</span></span> <span data-ttu-id="da2a2-364">Isso ignora as configurações de expiração deslizante configuradas anteriormente.</span><span class="sxs-lookup"><span data-stu-id="da2a2-364">This ignores any sliding expiration settings previously configured.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="da2a2-365">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="da2a2-365">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true,
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="da2a2-366">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="da2a2-366">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true,
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

---

## <a name="see-also"></a><span data-ttu-id="da2a2-367">Consulte também</span><span class="sxs-lookup"><span data-stu-id="da2a2-367">See also</span></span>

* [<span data-ttu-id="da2a2-368">Alterações de autenticação 2.0 / Comunicado de migração</span><span class="sxs-lookup"><span data-stu-id="da2a2-368">Auth 2.0 Changes / Migration Announcement</span></span>](https://github.com/aspnet/Announcements/issues/262)
* [<span data-ttu-id="da2a2-369">Limitando a identidade por esquema</span><span class="sxs-lookup"><span data-stu-id="da2a2-369">Limiting identity by scheme</span></span>](xref:security/authorization/limitingidentitybyscheme)
