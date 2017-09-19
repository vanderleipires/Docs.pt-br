---
title: Configurar a identidade do ASP.NET Core
author: AdrienTorris
description: "Entender os valores padrão de identidade do ASP.NET Core e configure as várias propriedades de identidade para usar valores personalizados."
keywords: "Autenticação do ASP.NET Core, identidade, segurança"
ms.author: scaddie
manager: wpickett
ms.date: 09/18/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity-configuration
ms.openlocfilehash: adf577ae1e1c752c3b1a332ec94a7a7627a7a4b4
ms.sourcegitcommit: 76d42f09f3e0dd2f2105493eca6b29994aa47706
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/19/2017
---
# <a name="configure-identity"></a>Configurar identidade

A identidade do ASP.NET Core tem alguns comportamentos padrão que você pode substituir facilmente em seu aplicativo `Startup` classe.

## <a name="passwords-policy"></a>Política de senhas

Por padrão, a identidade requer que as senhas conter um caractere maiusculo, caractere minúsculo, um dígito e um caractere alfanumérico. Também há algumas outras restrições. Se você deseja simplificar a restrições de senha, você pode fazer isso no `Startup` classe do seu aplicativo.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

ASP.NET Core 2.0 adicionado o `RequiredUniqueChars` propriedade. Caso contrário, as opções são as mesmas do ASP.NET Core 1. x.

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

---

`IdentityOptions.Password`tem as seguintes propriedades:
* `RequireDigit`: Exige um número entre 0-9, a senha. Padrão é true.
* `RequiredLength`: O comprimento mínimo da senha. O padrão é 6.
* `RequireNonAlphanumeric`: Requer um caractere não alfanumérico na senha. Padrão é true.
* `RequireUppercase`: Requer um caractere em letra maiuscula na senha. Padrão é true.
* `RequireLowercase`: Requer um caractere minúsculo na senha. Padrão é true.
* `RequiredUniqueChars`: Requer o número de caracteres distintos na senha. O padrão é 1.


## <a name="users-lockout"></a>Bloqueio do usuário

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,39-42,50-52)]

`IdentityOptions.Lockout`tem as seguintes propriedades:
* `DefaultLockoutTimeSpan`: A quantidade de tempo de uma usuário é bloqueada quando ocorre um bloqueio. O padrão é 5 minutos.
* `MaxFailedAccessAttempts`: O número de tentativas de acesso com falha até que um usuário seja bloqueado, se o bloqueio é ativado. O padrão é 5.
* `AllowedForNewUsers`: Determina se um novo usuário poderá ser bloqueado. Padrão é true.


## <a name="sign-in-settings"></a>Configurações de entrada

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)]

`IdentityOptions.SignIn`tem as seguintes propriedades:
* `RequireConfirmedEmail`: Requer um email confirmado para entrar. O padrão é false.
* `RequireConfirmedPhoneNumber`: Requer um número de telefone confirmado entrar. O padrão é false.


## <a name="user-validation-settings"></a>Configurações de validação do usuário

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,48-52)]

`IdentityOptions.User`tem as seguintes propriedades:
* `RequireUniqueEmail`: Exige que cada usuário tenha um email exclusivo. O padrão é false.
* `AllowedUserNameCharacters`: Caracteres permitidos no nome do usuário. O padrão é abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+.

## <a name="applications-cookie-settings"></a>Configurações de cookie do aplicativo

Como a política de senhas, todas as configurações de cookie do aplicativo podem ser alteradas no `Startup` classe.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Em `ConfigureServices` no `Startup` classe, você pode configurar o cookie do aplicativo.

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

--- 

`CookieAuthenticationOptions`tem as seguintes propriedades:
* `Cookie.Name`: O nome do cookie. O padrão é. AspNetCore.Cookies.
* `Cookie.HttpOnly`: Quando for verdadeiro, o cookie não é acessível a partir de scripts do lado do cliente. Padrão é true.
* `ExpireTimeSpan`: Controla o tempo que o tíquete de autenticação é armazenado no cookie permanecerá válido do ponto em que ele é criado. O padrão é 14 dias.
* `LoginPath`: Quando um usuário está autorizado, ele será redirecionado para esse caminho para fazer logon. O padrão é a conta/logon.
* `LogoutPath`: Quando um usuário é desconectado, ele será redirecionado para esse caminho. O padrão é a conta/logoff.
* `AccessDeniedPath`: Quando um usuário não é uma verificação de autorização, ele será redirecionado para esse caminho. O padrão é a conta/AccessDenied.
* `SlidingExpiration`: Quando for verdadeiro, será emitido um novo cookie com um novo tempo de expiração quando o cookie atual é a janela de validade máximo na metade. Padrão é true.
* `ReturnUrlParameter`: O ReturnUrlParameter determina o nome do parâmetro de cadeia de caracteres de consulta que é acrescentado pelo middleware quando um código de 401 status não autorizado é alterado para um redirecionamento 302 para o caminho de logon.
* `AuthenticationScheme`: Isso só é relevante para o ASP.NET Core 1. x. O nome lógico para um esquema de autenticação específico.
* `AutomaticAuthenticate`: Esse sinalizador só é relevante para o ASP.NET Core 1. x. Quando for verdadeiro, autenticação de cookie deve executar em cada solicitação e tente validar e reconstrua qualquer entidade de segurança serializada criado por ele.

