---
title: Configurar a identidade do ASP.NET Core
author: AdrienTorris
description: Entender valores padrão de identidade do ASP.NET Core e saber como configurar propriedades de identidade para usar valores personalizados.
ms.author: scaddie
ms.date: 03/06/2018
uid: security/authentication/identity-configuration
ms.openlocfilehash: 914e9b22ed52b560366fdff1f2430d3dd66454c3
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276248"
---
# <a name="configure-aspnet-core-identity"></a>Configurar a identidade do ASP.NET Core

Identidade do ASP.NET Core usa a configuração padrão para configurações como a política de senha, tempo de bloqueio e configurações de cookie. Essas configurações podem ser substituídas no aplicativo do `Startup` classe.

## <a name="identity-options"></a>Opções de identidade

O [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) classe representa as opções que podem ser usadas para configurar o sistema de identidade.

### <a name="claims-identity"></a>Identidade baseada em declarações

[IdentityOptions.ClaimsIdentity](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.claimsidentity) Especifica o [ClaimsIdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions) com as propriedades mostradas na tabela.

| Propriedade | Descrição | Padrão |
| -------- | ----------- | :-----: |
| [RoleClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.roleclaimtype) | Obtém ou define o tipo de declaração usado em uma declaração de função. | [ClaimTypes.Role](/dotnet/api/system.security.claims.claimtypes.role) |
| [SecurityStampClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.securitystampclaimtype) | Obtém ou define o tipo de declaração usado para a declaração de carimbo de segurança. | `AspNet.Identity.SecurityStamp` |
| [UserIdClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.useridclaimtype) | Obtém ou define o tipo de declaração usado para a declaração do identificador de usuário. | [ClaimTypes.NameIdentifier](/dotnet/api/system.security.claims.claimtypes.nameidentifier) |
| [UserNameClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.usernameclaimtype) | Obtém ou define o tipo de declaração usado para a declaração de nome de usuário. | [ClaimTypes.Name](/dotnet/api/system.security.claims.claimtypes.name) |

### <a name="lockout"></a>Bloqueio

Impeça o usuário para um período de tempo após um determinado número de tentativas de acesso com falha (padrão: bloqueio de 5 minutos após 5 tentativas de acesso). Uma autenticação bem-sucedida redefine a contagem de tentativas de acesso com falha e redefine o relógio.

O exemplo a seguir mostra os valores padrão:

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,39-42,50-52)]

Confirme se [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync) define `lockoutOnFailure` para `true`:

```csharp
var result = await _signInManager.PasswordSignInAsync(
                 Input.Email, Input.Password, Input.RememberMe, lockoutOnFailure: true);
```

[IdentityOptions.Lockout](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.lockout) Especifica o [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) com as propriedades mostradas na tabela.

| Propriedade | Descrição | Padrão |
| -------- | ----------- | :-----: |
| [AllowedForNewUsers](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.allowedfornewusers) | Determina se um novo usuário poderá ser bloqueado. | `true` |
| [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan) | A quantidade de tempo de uma usuário é bloqueada quando ocorre um bloqueio. | 5 minutos |
| [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) | O número de tentativas de acesso com falha até que um usuário seja bloqueado, se o bloqueio é ativado. | 5 |

### <a name="password"></a>Senha

Por padrão, a identidade requer que as senhas conter um caractere maiusculo, caractere minúsculo, um dígito e um caractere não alfanumérico. Senhas devem ter pelo menos seis caracteres. [PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) podem ser alterados no `Startup.ConfigureServices`.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

ASP.NET Core 2.0 adicionado o [RequiredUniqueChars](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireduniquechars) propriedade. Caso contrário, as opções são o mesmo que o ASP.NET Core 1. x.

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

---

[IdentityOptions.Password](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.password) Especifica o [PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) com as propriedades mostradas na tabela.

| Propriedade | Descrição | Padrão |
| -------- | ----------- | :-----: |
| [RequireDigit](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredigit) | Requer um número entre 0-9, a senha. | `true` |
| [RequiredLength](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredlength) | O comprimento mínimo da senha. | 6 |
| [RequiredUniqueChars](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireduniquechars) | Só se aplica ao ASP.NET Core 2.0 ou posterior.<br><br> Requer o número de caracteres distintos na senha. | 1 |
| [RequireLowercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirelowercase) | Requer um caractere minúsculo na senha. | `true` |
| [RequireNonAlphanumeric](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirenonalphanumeric) | Exige um caractere não alfanumérico na senha. | `true` |
| [RequireUppercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireuppercase) | Requer um caractere maiusculo na senha. | `true` |

### <a name="sign-in"></a>entrar

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)]

[IdentityOptions.SignIn](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.signin) Especifica o [SignInOptions](/dotnet/api/microsoft.aspnetcore.identity.signinoptions) com as propriedades mostradas na tabela.

| Propriedade | Descrição | Padrão |
| -------- | ----------- | :-----: |
| [RequireConfirmedEmail](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedemail) | Requer um email confirmado para entrar. | `false` |
| [RequireConfirmedPhoneNumber](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedphonenumber) | Requer um número de telefone confirmado entrar. | `false` |

### <a name="tokens"></a>Tokens

[IdentityOptions.Tokens](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.tokens) Especifica o [TokenOptions](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions) com as propriedades mostradas na tabela.


|                                                        Propriedade                                                         |                                                                                      Descrição                                                                                      |
|-------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     [AuthenticatorTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.authenticatortokenprovider)     |                                       Obtém ou define o `AuthenticatorTokenProvider` usado para validar entradas de dois fatores com um autenticador.                                       |
|       [ChangeEmailTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changeemailtokenprovider)       |                                     Obtém ou define o `ChangeEmailTokenProvider` usado para gerar tokens usados nos emails de confirmação de alteração de email.                                     |
| [ChangePhoneNumberTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changephonenumbertokenprovider) |                                      Obtém ou define o `ChangePhoneNumberTokenProvider` usado para gerar tokens usados ao alterar os números de telefone.                                      |
| [EmailConfirmationTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.emailconfirmationtokenprovider) |                                             Obtém ou define o provedor de token usado para gerar tokens usados nos emails de confirmação de conta.                                              |
|     [PasswordResetTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.passwordresettokenprovider)     | Obtém ou define o [IUserTwoFactorTokenProvider<TUser> ](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactortokenprovider-1) usado para gerar tokens usados nos emails de redefinição de senha. |
|                    [ProviderMap](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.providermap)                    |                Usado para construir um [provedor de Token de usuário](/dotnet/api/microsoft.aspnetcore.identity.tokenproviderdescriptor) com a chave usada como o nome do provedor.                 |

### <a name="user"></a>User

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,48-52)]

[IdentityOptions.User](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.user) Especifica o [UserOptions](/dotnet/api/microsoft.aspnetcore.identity.useroptions) com as propriedades mostradas na tabela.

| Propriedade | Descrição | Padrão |
| -------- | ----------- | :-----: |
| [AllowedUserNameCharacters](/dotnet/api/microsoft.aspnetcore.identity.useroptions.allowedusernamecharacters) | Caracteres permitidos no nome do usuário. | abcdefghijklmnopqrstuvwxyz<br>ABCDEFGHIJKLMNOPQRSTUVWXYZ<br>0123456789<br>-._@+ |
| [RequireUniqueEmail](/dotnet/api/microsoft.aspnetcore.identity.useroptions.requireuniqueemail) | Requer que cada usuário tenha um email exclusivo. | `false` |

## <a name="cookie-settings"></a>Configurações de cookie

Configurar o cookie do aplicativo no `Startup.ConfigureServices`:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

---

[CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions) tem as seguintes propriedades:

|                                                               Propriedade                                                               |                                                                                                                                                           Descrição                                                                                                                                                            |
|--------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath)       |                                                                 Informa o manipulador que deve alterar uma saída <em>403 Proibido</em> código de status em um <em>redirecionamento 302</em> no caminho especificado.<br><br>O valor padrão é `/Account/AccessDenied`.                                                                  |
|             [authenticationScheme](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.authenticationscheme)              |                                                                                                                Aplica-se somente a ASP.NET Core 1. x.<br><br> O nome lógico para um esquema de autenticação específico.                                                                                                                |
|            [AutomaticAuthenticate](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticauthenticate)             |                                                                       Aplica-se somente a ASP.NET Core 1. x.<br><br> Quando for verdadeiro, autenticação de cookie deve executar em cada solicitação e tente validar e reconstrua qualquer entidade de segurança serializada criado por ele.                                                                        |
|               [AutomaticChallenge](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticchallenge)                |                                              Aplica-se somente a ASP.NET Core 1. x.<br><br> Se for true, o middleware de autenticação manipula desafios automática. Se false, o middleware de autenticação altera somente respostas quando explicitamente indicado pelo `AuthenticationScheme`.                                               |
|               [ClaimsIssuer](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.claimsissuer)               |                                                             Obtém ou define o emissor deve ser usado para quaisquer declarações que são criadas (herdado de [AuthenticationSchemeOptions](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions)).                                                             |
|                             [Cookie.Domain](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain)                              |                                                                                                                                             O domínio para associar o cookie.                                                                                                                                             |
|                         [Cookie.Expiration](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.expiration)                          |                 Obtém ou define o tempo de vida do cookie HTTP (não o cookie de autenticação). Esta propriedade é substituída por [ExpireTimeSpan](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.expiretimespan). Ele não deve ser usado no contexto de CookieAuthentication.                  |
|                           [Cookie.HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly)                            |                                                                                                               Indica se um cookie pode ser acessado pelo script do lado do cliente.<br><br>O valor padrão é `true`.                                                                                                                |
|                               [Cookie.Name](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name)                                |                                                                                                                            O nome do cookie.<br><br>O valor padrão é `.AspNetCore.Cookies`.                                                                                                                            |
|                               [Cookie.Path](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path)                                |                                                                                                                                                         O caminho do cookie.                                                                                                                                                         |
|                           [Cookie.SameSite](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite)                            |                                                                                           O `SameSite` atributo do cookie.<br><br>O valor padrão é [SameSiteMode.Lax](/dotnet/api/microsoft.aspnetcore.http.samesitemode).                                                                                            |
|                       [Cookie.SecurePolicy](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.securepolicy)                        |                                                   O [CookieSecurePolicy](/dotnet/api/microsoft.aspnetcore.http.cookiesecurepolicy) configuração.<br><br>O valor padrão é [CookieSecurePolicy.SameAsRequest](/dotnet/api/microsoft.aspnetcore.http.cookiesecurepolicy).                                                    |
|                  [CookieDomain](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiedomain)                   |                                                                                                                      Aplica-se somente a ASP.NET Core 1. x.<br><br> O nome de domínio em que o cookie é atendido.                                                                                                                       |
|                [CookieHttpOnly](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiehttponly)                 |                                                                                       Aplica-se somente a ASP.NET Core 1. x.<br><br> Um sinalizador que indica se o cookie deve ser acessível somente para servidores.<br><br>O valor padrão é `true`.                                                                                        |
|                    [CookiePath](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiepath)                     |                                                                                                                  Aplica-se somente a ASP.NET Core 1. x.<br><br> Usado para isolar os aplicativos em execução no mesmo nome de host.                                                                                                                   |
|                  [CookieSecure](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiesecure)                   | Aplica-se somente a ASP.NET Core 1. x.<br><br> Um sinalizador que indica se o cookie criado deve ser limitado para HTTPS (`CookieSecurePolicy.Always`), HTTP ou HTTPS (`CookieSecurePolicy.None`), ou o mesmo protocolo que a solicitação (`CookieSecurePolicy.SameAsRequest`).<br><br>O valor padrão é `CookieSecurePolicy.SameAsRequest`. |
|          [CookieManager](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.cookiemanager)          |                                                                                                                         O componente usado para obter cookies de solicitação ou defini-las na resposta.                                                                                                                          |
| [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider) |                                                                             Se definido, o provedor usado pelo [CookieAuthenticationHandler](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationhandler) para proteção de dados.                                                                             |
|                      [Descrição](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.description)                       |                                                                                                Aplica-se somente a ASP.NET Core 1. x.<br><br> Informações adicionais sobre o tipo de autenticação que é disponibilizado para o aplicativo.                                                                                                |
|                 [Eventos](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.events)                 |                                                                                                      O manipulador chama métodos no provedor que dê o controle de aplicativo em determinados pontos onde ocorre o processamento.                                                                                                       |
|                 [EventsType](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.eventstype)                 |                                                            Se definido, o tipo de serviço para obter o `Events` instância em vez da propriedade (herdado de [AuthenticationSchemeOptions](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions)).                                                            |
|         [ExpireTimeSpan](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.expiretimespan)         |                                                                                      Controles de quanto tempo o tíquete de autenticação armazenado no cookie permanece válido desde o ponto em que ele é criado.<br><br>O valor padrão é 14 dias.                                                                                       |
|              [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath)              |                                                                                                       Quando um usuário está autorizado, eles são redirecionados para esse caminho para fazer logon.<br><br>O valor padrão é `/Account/Login`.                                                                                                       |
|             [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath)             |                                                                                                            Quando um usuário é desconectado, eles serão redirecionados a esse caminho.<br><br>O valor padrão é `/Account/Logout`.                                                                                                            |
|     [ReturnUrlParameter](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.returnurlparameter)     |                                              Determina o nome do parâmetro de cadeia de caracteres de consulta que é acrescentado pelo middleware quando um <em>401 não autorizado</em> código de status é alterado para um <em>redirecionamento 302</em> até o caminho de logon.<br><br>O valor padrão é `ReturnUrl`.                                              |
|           [SessionStore](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.sessionstore)           |                                                                                                                              Um contêiner opcional no qual armazenar a identidade entre solicitações.                                                                                                                               |
|      [slidingExpiration](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.slidingexpiration)      |                                                                           Quando for verdadeiro, um novo cookie é emitido com uma nova hora de expiração quando o cookie atual é mais de meio a janela de expiração.<br><br>O valor padrão é `true`.                                                                           |
|       [TicketDataFormat](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.ticketdataformat)       |                                                                                                 O `TicketDataFormat` é usado para proteger e desproteger a identidade e outras propriedades que são armazenadas no valor do cookie.                                                                                                  |

