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
ms.openlocfilehash: 6279d3b4ac3be102449089dc66eeeb0495cfc4c0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
# <a name="using-cookie-authentication-without-aspnet-core-identity"></a>Usando a autenticação de Cookie sem identidade do ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT) e [Luke Latham](https://github.com/guardrex)

Como você viu nos tópicos anteriores de autenticação, [a identidade do ASP.NET Core](xref:security/authentication/identity) é um provedor de autenticação completa e completo para criar e manter os logons. No entanto, você talvez queira usar sua própria lógica de autenticação personalizada com autenticação baseada em cookie às vezes. Você pode usar a autenticação baseada em cookie como um provedor de autenticação autônomo sem a identidade do ASP.NET Core.

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/cookie/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

Para obter informações sobre a autenticação baseada em cookie Migrando do ASP.NET Core 1. x para 2.0, consulte [migrando autenticação e identidade de tópico do ASP.NET Core 2.0 (autenticação baseada em Cookie)](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication).

## <a name="configuration"></a>Configuração

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Se você não estiver usando o [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), instale a versão 2.0 + o [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) pacote NuGet.

No `ConfigureServices` método, criar o serviço de Middleware de autenticação com o `AddAuthentication` e `AddCookie` métodos:

[!code-csharp[Main](cookie/sample/Startup.cs?name=snippet1)]

`AuthenticationScheme`passado para `AddAuthentication` define o esquema de autenticação padrão para o aplicativo. `AuthenticationScheme`é útil quando há várias instâncias de autenticação de cookie e você deseja [autorizar com um esquema específico](xref:security/authorization/limitingidentitybyscheme). Definindo o `AuthenticationScheme` para `CookieAuthenticationDefaults.AuthenticationScheme` fornece um valor de "Cookies" para o esquema. Você pode fornecer qualquer valor de cadeia de caracteres que diferencia o esquema.

No `Configure` método, use o `UseAuthentication` método a invocar o Middleware de autenticação que define o `HttpContext.User` propriedade. Chamar o `UseAuthentication` método antes de chamar `AddMvcWithDefaultRoute` em um aplicativo MVC ou `AddMvc` em um aplicativo de páginas Razor:

[!code-csharp[Main](cookie/sample/Startup.cs?name=snippet2)]

**Opções de AddCookie**

O [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0) classe é usada para configurar as opções de provedor de autenticação.

| Opção | Descrição |
| ------ | ----------- |
| [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath?view=aspnetcore-2.0) | Fornece o caminho para fornecer um 302 não encontrado (redirecionamento de URL) quando disparadas por `HttpContext.ForbidAsync`. O valor padrão é `/Account/AccessDenied`. |
| [ClaimsIssuer](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.claimsissuer?view=aspnetcore-2.0) | O emissor a ser usado para o [emissor](/dotnet/api/system.security.claims.claim.issuer) propriedade em quaisquer declarações criado pelo serviço de autenticação de cookie. |
| [Cookie.Domain](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain?view=aspnetcore-2.0) | O nome de domínio em que o cookie é atendido. Por padrão, esse é o nome do host da solicitação. O navegador envia somente o cookie em solicitações para um nome de host correspondente. Talvez você queira ajustar isso para que os cookies disponíveis para qualquer host no seu domínio. Por exemplo, definir o domínio do cookie `.contoso.com` disponibiliza para `contoso.com`, `www.contoso.com`, e `staging.www.contoso.com`. |
| [Cookie.Expiration](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.expiration?view=aspnetcore-2.0) | Obtém ou define o tempo de vida de um cookie. No momento, essa opção não ops e se torna obsoleta no ASP.NET Core 2.1 +. Use o `ExpireTimeSpan` opção para definir a expiração do cookie. Para obter mais informações, consulte [esclarecer o comportamento de CookieAuthenticationOptions.Cookie.Expiration](https://github.com/aspnet/Security/issues/1293). |
| [Cookie.HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly?view=aspnetcore-2.0) | Um sinalizador que indica se o cookie deve ser acessível somente para servidores. A alteração desse valor para `false` permite que scripts do lado do cliente para acessar o cookie e pode abrir o aplicativo para roubo de cookie deve ter seu aplicativo um [script entre sites (XSS)](xref:security/cross-site-scripting) vulnerabilidade. O valor padrão é `true`. |
| [Cookie.Name](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name?view=aspnetcore-2.0) | Define o nome do cookie. |
| [Cookie.Path](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path?view=aspnetcore-2.0) | Usado para isolar os aplicativos em execução no mesmo nome de host. Se você tiver um aplicativo em execução no `/app1` e deseja restringir os cookies para esse aplicativo, defina o `CookiePath` propriedade `/app1`. Por isso, o cookie só está disponível em solicitações para `/app1` e qualquer aplicativo abaixo dele. |
| [Cookie.SameSite](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite?view=aspnetcore-2.0) | Indica se o navegador deve permitir que o cookie a ser anexado ao mesmo site solicitações somente (`SameSiteMode.Strict`) ou solicitações de sites usando métodos HTTP seguros e solicitações do mesmo site (`SameSiteMode.Lax`). Quando definido como `SameSiteMode.None`, o valor do cabeçalho de cookie não está definido. Observe que [Middleware do Cookie política](#cookie-policy-middleware) pode substituir o valor que você fornecer. Para dar suporte à autenticação OAuth, o valor padrão é `SameSiteMode.Lax`. Para obter mais informações, consulte [autenticação OAuth interrompida devido à política de cookie SameSite](https://github.com/aspnet/Security/issues/1231). |
| [Cookie.SecurePolicy](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.securepolicy?view=aspnetcore-2.0) | Um sinalizador que indica se o cookie criado deve ser limitado para HTTPS (`CookieSecurePolicy.Always`), HTTP ou HTTPS (`CookieSecurePolicy.None`), ou o mesmo protocolo que a solicitação (`CookieSecurePolicy.SameAsRequest`). O valor padrão é `CookieSecurePolicy.SameAsRequest`. |
| [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0) | Conjuntos de `DataProtectionProvider` que é usado para criar o padrão `TicketDataFormat`. Se o `TicketDataFormat` propriedade for definida, o `DataProtectionProvider` opção não é usada. Se não for fornecido, o provedor de proteção de dados do aplicativo padrão é usado. |
| [Eventos](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.events?view=aspnetcore-2.0) | O manipulador chama métodos no provedor que dê o controle de aplicativo em determinados pontos de processamento. Se `Events` não fornecido, uma instância padrão é fornecida que não faz nada quando os métodos são chamados. |
| [EventsType](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.eventstype?view=aspnetcore-2.0) | Usado como o tipo de serviço para obter o `Events` instância em vez da propriedade. |
| [ExpireTimeSpan](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.expiretimespan?view=aspnetcore-2.0) | O `TimeSpan` depois que o tíquete de autenticação armazenado dentro o cookie expira. `ExpireTimeSpan`é adicionado à hora atual para criar o tempo de expiração do tíquete de. O `ExpiredTimeSpan` valor sempre vai para o AuthTicket criptografado verificado pelo servidor. Ele também pode ir para o [Set-Cookie](https://tools.ietf.org/html/rfc6265#section-4.1) cabeçalho, mas somente se `IsPersistent` está definido. Para definir `IsPersistent` para `true`, configure o [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties) passado para `SignInAsync`. O valor padrão de `ExpireTimeSpan` é de 14 dias. |
| [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath?view=aspnetcore-2.0) | Fornece o caminho para fornecer um 302 não encontrado (redirecionamento de URL) quando disparadas por `HttpContext.ChallengeAsync`. A URL atual que gerou o 401 é adicionada para o `LoginPath` como um parâmetro de cadeia de caracteres de consulta nomeado pelo `ReturnUrlParameter`. Uma vez uma solicitação para o `LoginPath` concede uma nova identidade, o `ReturnUrlParameter` valor é usado para redirecionar o navegador para a URL que fez com que o código de status não autorizado original. O valor padrão é `/Account/Login`. |
| [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath?view=aspnetcore-2.0) | Se o `LogoutPath` é fornecido para o manipulador, em seguida, redireciona uma solicitação para o caminho com base no valor da `ReturnUrlParameter`. O valor padrão é `/Account/Logout`. |
| [ReturnUrlParameter](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.returnurlparameter?view=aspnetcore-2.0) | Determina o nome do parâmetro de cadeia de caracteres de consulta será anexado o manipulador de resposta 302 não encontrado (redirecionamento de URL). `ReturnUrlParameter`é usado quando uma solicitação chega no `LoginPath` ou `LogoutPath` para retornar o navegador a URL original depois que a ação de logon ou logout é executada. O valor padrão é `ReturnUrl`. |
| [SessionStore](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.sessionstore?view=aspnetcore-2.0) | Um contêiner opcional usado para armazenar a identidade entre solicitações. Quando usado, um identificador de sessão é enviado ao cliente. `SessionStore`pode ser usado para atenuar problemas potenciais com identidades grandes. |
| [SlidingExpiration](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-2.0) | Um sinalizador que indica se um novo cookie com um tempo de expiração atualizados deve ser emitido dinamicamente. Isso pode acontecer em qualquer solicitação em que o período de expiração do cookie atual mais de 50% expirou. A nova data de expiração é movida para frente para ser a data atual mais o `ExpireTimespan`. Um [tempo de expiração do cookie absoluto](xref:security/authentication/cookie#absolute-cookie-expiration) pode ser definida usando o `AuthenticationProperties` classe ao chamar `SignInAsync`. Um tempo de expiração absoluta pode melhorar a segurança do seu aplicativo, limitando a quantidade de tempo que o cookie de autenticação é válido. O valor padrão é `true`. |
| [TicketDataFormat](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.ticketdataformat?view=aspnetcore-2.0) | O `TicketDataFormat` é usado para proteger e desproteger a identidade e outras propriedades que são armazenadas no valor do cookie. Se não for fornecido, um `TicketDataFormat` é criado usando o [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0). |
| [Validar](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.validate?view=aspnetcore-2.0) | Método que verifica que as opções são válidas. |

Definir `CookieAuthenticationOptions` na configuração de serviço para autenticação no `ConfigureServices` método:

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

ASP.NET Core 1. x usa cookie [middleware](xref:fundamentals/middleware) que serializa um objeto de usuário em um cookie criptografado. Em solicitações subsequentes, o cookie é validado, e a entidade de segurança é recriada e atribuída ao `HttpContext.User` propriedade.

Instalar o [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) pacote NuGet em seu projeto. Este pacote contém o middleware de cookie.

Use o `UseCookieAuthentication` método o `Configure` método no seu *Startup.cs* arquivo antes de `UseMvc` ou `UseMvcWithDefaultRoute`:

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

**Opções de CookieAuthenticationOptions**

O [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions?view=aspnetcore-1.1) classe é usada para configurar as opções de provedor de autenticação.

| Opção | Descrição |
| ------ | ----------- |
| [AuthenticationScheme](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.authenticationscheme?view=aspnetcore-1.1) | Define o esquema de autenticação. `AuthenticationScheme`é útil quando há várias instâncias de autenticação e você deseja [autorizar com um esquema específico](xref:security/authorization/limitingidentitybyscheme). Definindo o `AuthenticationScheme` para `CookieAuthenticationDefaults.AuthenticationScheme` fornece um valor de "Cookies" para o esquema. Você pode fornecer qualquer valor de cadeia de caracteres que diferencia o esquema. |
| [AutomaticAuthenticate](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticauthenticate?view=aspnetcore-1.1) | Define um valor para indicar que a autenticação de cookie deve executar em cada solicitação e tente validar e reconstrua qualquer entidade de segurança serializada criado por ele. |
| [AutomaticChallenge](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticchallenge?view=aspnetcore-1.1) | Se for true, o middleware de autenticação manipula desafios automática. Se false, o middleware de autenticação altera somente respostas quando explicitamente indicado pelo `AuthenticationScheme`. |
| [ClaimsIssuer](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.claimsissuer?view=aspnetcore-1.1) | O emissor a ser usado para o [emissor](/dotnet/api/system.security.claims.claim.issuer) propriedade em quaisquer declarações criado pelo middleware de autenticação de cookie. |
| [CookieDomain](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiedomain?view=aspnetcore-1.1) | O nome de domínio em que o cookie é atendido. Por padrão, esse é o nome do host da solicitação. O navegador serve apenas o cookie para um nome de host correspondente. Talvez você queira ajustar isso para que os cookies disponíveis para qualquer host no seu domínio. Por exemplo, definir o domínio do cookie `.contoso.com` disponibiliza para `contoso.com`, `www.contoso.com`, e `staging.www.contoso.com`. |
| [CookieHttpOnly](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiehttponly?view=aspnetcore-1.1) | Um sinalizador que indica se o cookie deve ser acessível somente para servidores. A alteração desse valor para `false` permite que scripts do lado do cliente para acessar o cookie e pode abrir o aplicativo para roubo de cookie deve ter seu aplicativo um [script entre sites (XSS)](xref:security/cross-site-scripting) vulnerabilidade. O valor padrão é `true`. |
| [CookiePath](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiepath?view=aspnetcore-1.1) | Usado para isolar os aplicativos em execução no mesmo nome de host. Se você tiver um aplicativo em execução no `/app1` e deseja restringir os cookies para esse aplicativo, defina o `CookiePath` propriedade `/app1`. Por isso, o cookie só está disponível em solicitações para `/app1` e qualquer aplicativo abaixo dele. |
| [CookieSecure](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiesecure?view=aspnetcore-1.1) | Um sinalizador que indica se o cookie criado deve ser limitado para HTTPS (`CookieSecurePolicy.Always`), HTTP ou HTTPS (`CookieSecurePolicy.None`), ou o mesmo protocolo que a solicitação (`CookieSecurePolicy.SameAsRequest`). O valor padrão é `CookieSecurePolicy.SameAsRequest`. |
| [Descrição](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.description?view=aspnetcore-1.1) | Informações adicionais sobre o tipo de autenticação que é disponibilizado para o aplicativo. |
| [ExpireTimeSpan](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.expiretimespan?view=aspnetcore-1.1) | O `TimeSpan` depois que o tíquete de autenticação expira. Ele é adicionado para a hora atual para criar o tempo de expiração do tíquete de. Para usar `ExpireTimeSpan`, você deve definir `IsPersistent` para `true` no `AuthenticationProperties` passado para `SignInAsync`. O valor padrão é 14 dias. |
| [SlidingExpiration](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-1.1) | Um sinalizador que indica se a data de expiração do cookie redefine quando houver mais de metade do `ExpireTimeSpan` do intervalo. A nova hora de exipiration é movida para frente para ser a data atual mais o `ExpireTimespan`. Um [tempo de expiração do cookie absoluto](xref:security/authentication/cookie#absolute-cookie-expiration) pode ser definida usando o `AuthenticationProperties` classe ao chamar `SignInAsync`. Um tempo de expiração absoluta pode melhorar a segurança do seu aplicativo, limitando a quantidade de tempo que o cookie de autenticação é válido. O valor padrão é `true`. |

Definir `CookieAuthenticationOptions` para o Middleware de autenticação de Cookie no `Configure` método:

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    ...
});
```

---

## <a name="cookie-policy-middleware"></a>Middleware de política de cookie

[Middleware do cookie política](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware) habilita recursos de política de cookie em um aplicativo. Adicionar o middleware para o pipeline de processamento de aplicativos é a ordem e minúsculas; ela afeta apenas os componentes registrados depois no pipeline.

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

 O [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) fornecido para o Middleware do Cookie política permitem que você controle características globais do processamento de cookie e gancho em manipuladores de processamento do cookie quando os cookies são anexados ou excluídos.

| Propriedade | Descrição |
| -------- | ----------- |
| [HttpOnly](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.httponly) | Afeta se os cookies devem ser HttpOnly, que é um sinalizador que indica se o cookie deve ser acessível somente para servidores. O valor padrão é `HttpOnlyPolicy.None`. |
| [MinimumSameSitePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.minimumsamesitepolicy) | Afeta o atributo de mesmo site do cookie (veja abaixo). O valor padrão é `SameSiteMode.Lax`. Essa opção está disponível para o ASP.NET Core 2.0 +. |
| [OnAppendCookie](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.onappendcookie) | Chamado quando um cookie é anexado. |
| [OnDeleteCookie](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.ondeletecookie) | Chamado quando um cookie é excluído. |
| [Proteger](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.secure) | Afeta se os cookies devem ser seguro. O valor padrão é `CookieSecurePolicy.None`. |

**MinimumSameSitePolicy** (ASP.NET Core 2.0 + apenas)

O padrão `MinimumSameSitePolicy` valor é `SameSiteMode.Lax` para permitir a autenticação OAuth2. Estritamente impor uma política do mesmo site do `SameSiteMode.Strict`, defina o `MinimumSameSitePolicy`. Embora essa configuração quebras OAuth2 e outros esquemas de autenticação entre origens, ele eleva o nível de segurança do cookie para outros tipos de aplicativos que não dependem de processamento de solicitação entre origens.

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

A configuração de Middleware de política de Cookie para `MinimumSameSitePolicy` pode afetar sua configuração de `Cookie.SameSite` na `CookieAuthenticationOptions` configurações de acordo com a matriz abaixo.

| MinimumSameSitePolicy | Cookie.SameSite | Configuração de Cookie.SameSite resultante |
| --------------------- | --------------- | --------------------------------- |
| SameSiteMode.None     | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict |
| SameSiteMode.Lax      | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.Lax<br>SameSiteMode.Lax<br>SameSiteMode.Strict |
| SameSiteMode.Strict   | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.Strict<br>SameSiteMode.Strict<br>SameSiteMode.Strict |

## <a name="creating-an-authentication-cookie"></a>Criando um cookie de autenticação

Para criar um cookie contendo informações de usuário, você precisa construir uma [ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal). As informações do usuário são serializadas e armazenadas no cookie. 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Chamar [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0) para assinar o usuário:

```csharp
await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme, 
    new ClaimsPrincipal(claimsIdentity));
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Chamar [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1) para assinar o usuário:

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity));
```

---

`SignInAsync`cria um cookie criptografado e o adiciona à resposta atual. Se você não especificar um `AuthenticationScheme`, o esquema padrão é usado.

Nos bastidores, a criptografia é ASP.NET Core [proteção de dados](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) sistema. Se você estiver hospedando o aplicativo em vários computadores, balanceamento de carga em aplicativos ou usando uma web farm, você deve [configurar a proteção de dados](xref:security/data-protection/configuration/overview) para usar o mesmo anel de chave e o identificador do aplicativo.

## <a name="signing-out"></a>Sair

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Para desconectar o usuário atual e exclua seus cookies, chame [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):

```csharp
await HttpContext.SignOutAsync(
    CookieAuthenticationDefaults.AuthenticationScheme);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Para desconectar o usuário atual e exclua seus cookies, chame [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):

```csharp
await HttpContext.Authentication.SignOutAsync(
    CookieAuthenticationDefaults.AuthenticationScheme);
```

---

Se você não estiver usando `CookieAuthenticationDefaults.AuthenticationScheme` (ou "Cookies") como o esquema (por exemplo, "ContosoCookie"), forneça o esquema usado ao configurar o provedor de autenticação. Caso contrário, o esquema padrão será usado.

## <a name="reacting-to-back-end-changes"></a>Reagir a alterações de back-end

Quando um cookie é criado, ele se torna a única fonte de identidade. Mesmo se você desabilitar um usuário nos seus sistemas de back-end, o sistema de autenticação de cookie não tem conhecimento sobre isso e um usuário permanece conectado como o cookie é válido.

O [ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal) evento no ASP.NET Core 2. x ou [ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1) método no núcleo do ASP.NET 1. x pode ser usado para interceptar e ignorar a validação da identidade do cookie. Isso reduz o risco dos usuários revogados acessando o aplicativo.

Uma abordagem para validação de cookie baseia-se em manter o controle de quando o banco de dados do usuário foi alterado. Se o banco de dados não foi alterado desde que o cookie do usuário foi emitido, não é necessário para autenticar o usuário novamente se o cookie ainda é válido. Para implementar este cenário, o banco de dados, que é implementado no `IUserRepository` para este exemplo armazena um `LastChanged` valor. Quando nenhum usuário é atualizado no banco de dados, o `LastChanged` valor é definido para a hora atual.

Para invalidar um cookie quando as alterações do banco de dados com base no `LastChanged` valor, criar o cookie com um `LastChanged` declaração contendo atual `LastChanged` valor do banco de dados:

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

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Para implementar uma substituição para o `ValidatePrincipal` evento, a gravação de um método com a seguinte assinatura em uma classe que deriva de [CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

Um exemplo é semelhante ao seguinte:

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

Registrar a instância de eventos durante o registro do serviço de cookie no `ConfigureServices` método. Forneça um registro de serviço com escopo definido para seu `CustomCookieAuthenticationEvents` classe:

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Para implementar uma substituição para o `ValidateAsync` evento, a gravação de um método com a seguinte assinatura:

```csharp
ValidateAsync(CookieValidatePrincipalContext)
```

A identidade do ASP.NET Core implementa essa verificação como parte de seu [SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync). Um exemplo é semelhante ao seguinte:

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

Registrar o evento durante a configuração de autenticação de cookie no `Configure` método:

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

Considere uma situação em que o nome do usuário é atualizado &mdash; uma decisão que não afeta a segurança de qualquer forma. Se você quiser atualizar não destrutivo a entidade de usuário, chame `context.ReplacePrincipal` e defina o `context.ShouldRenew` propriedade `true`.

> [!WARNING]
> A abordagem descrita aqui é acionada em cada solicitação. Isso pode resultar em uma penalidade de desempenho para o aplicativo.

## <a name="persistent-cookies"></a>Cookies persistentes

Talvez você queira que o cookie para persistir nas sessões do navegador. Essa persistência deve ser habilitada apenas com a permissão explícita do usuário com uma "Lembrar-Me" caixa de seleção no logon ou um mecanismo semelhante. 

O trecho de código a seguir cria uma identidade e o cookie correspondente que sobrevive a fechamentos de navegador. As configurações de expiração deslizante configuradas anteriormente são respeitadas. Se o cookie expirar enquanto o navegador for fechado, o navegador limpa o cookie depois que ele seja reiniciado.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

O [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties?view=aspnetcore-2.0) classe reside no `Microsoft.AspNetCore.Authentication` namespace.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

O [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.http.authentication.authenticationproperties?view=aspnetcore-1.1) classe reside no `Microsoft.AspNetCore.Http.Authentication` namespace.

---

## <a name="absolute-cookie-expiration"></a>Expiração do cookie absoluto

Você pode definir um tempo de expiração absoluta com `ExpiresUtc`. Você também deve definir `IsPersistent`; caso contrário, `ExpiresUtc` é ignorada e um cookie de sessão único é criado. Quando `ExpiresUtc` é definido em `SignInAsync`, ela substitui o valor da `ExpireTimeSpan` opção de `CookieAuthenticationOptions`, se definido.

O trecho de código a seguir cria uma identidade e o cookie correspondente que tem duração de 20 minutos. Isso ignora as configurações de expiração deslizante configuradas anteriormente.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

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

## <a name="see-also"></a>Consulte também

* [Alterações de autenticação 2.0 / Comunicado de migração](https://github.com/aspnet/Announcements/issues/262)
* [Limitando a identidade por esquema](xref:security/authorization/limitingidentitybyscheme)
