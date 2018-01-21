---
title: Configurar a identidade do ASP.NET Core
author: AdrienTorris
description: "Entender os valores padrão de identidade do ASP.NET Core e configure as várias propriedades de identidade para usar valores personalizados."
ms.author: scaddie
manager: wpickett
ms.date: 01/11/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity-configuration
ms.openlocfilehash: d3a13d1cef3417522460b44c52c1361c3e9d1162
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/19/2018
---
# <a name="configure-identity"></a>Configurar identidade

A identidade do ASP.NET Core tem comportamentos comuns em aplicativos, como a política de senha, tempo de bloqueio e configurações de cookie que você pode substituir facilmente em seu aplicativo `Startup` classe.

## <a name="passwords-policy"></a>Política de senhas

Por padrão, a identidade requer que as senhas conter um caractere maiusculo, caractere minúsculo, um dígito e um caractere não alfanumérico. Também há algumas outras restrições. Para simplificar as restrições de senha, modifique o `ConfigureServices` método o `Startup` classe do seu aplicativo.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

ASP.NET Core 2.0 adicionado o `RequiredUniqueChars` propriedade. Caso contrário, as opções são as mesmas do ASP.NET Core 1. x.

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

---

`IdentityOptions.Password`tem as seguintes propriedades:

| Propriedade                | Descrição                       | Padrão |
| ----------------------- | --------------------------------- | ------- |
| `RequireDigit`          | Requer um número entre 0-9, a senha. | true |
| `RequiredLength`        | O comprimento mínimo da senha. | 6 |
| `RequireNonAlphanumeric`| Exige um caractere não alfanumérico na senha. | true |
| `RequireUppercase`      | Requer um caractere em letra maiuscula na senha. | true |
| `RequireLowercase`      | Requer um caractere minúsculo na senha. | true |
| `RequiredUniqueChars`   | Requer o número de caracteres distintos na senha. | 1 |


## <a name="users-lockout"></a>Bloqueio do usuário

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,39-42,50-52)]

`IdentityOptions.Lockout`tem as seguintes propriedades:

| Propriedade                | Descrição                       | Padrão |
| ----------------------- | --------------------------------- | ------- |
| `DefaultLockoutTimeSpan` | A quantidade de tempo de uma usuário é bloqueada quando ocorre um bloqueio.  | 5 minutos  |
| `MaxFailedAccessAttempts` | O número de tentativas de acesso com falha até que um usuário seja bloqueado, se o bloqueio é ativado.  | 5 |
| `AllowedForNewUsers` | Determina se um novo usuário poderá ser bloqueado.  | true |

## <a name="sign-in-settings"></a>Configurações de entrada

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)]

`IdentityOptions.SignIn`tem as seguintes propriedades:

| Propriedade                | Descrição                       | Padrão |
| ----------------------- | --------------------------------- | ------- |
| `RequireConfirmedEmail` | Requer um email confirmado para entrar. | false  |
| `RequireConfirmedPhoneNumber` |  Requer um número de telefone confirmado entrar. | false  |

## <a name="user-validation-settings"></a>Configurações de validação do usuário

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,48-52)]

`IdentityOptions.User`tem as seguintes propriedades:

| Propriedade                | Descrição                       | Padrão |
| ----------------------- | --------------------------------- | ------- |
| `RequireUniqueEmail`  | Requer que cada usuário tenha um email exclusivo. | false  |
| `AllowedUserNameCharacters`  | Caracteres permitidos no nome do usuário. | abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+ |


## <a name="applications-cookie-settings"></a>Configurações de cookie do aplicativo

Como a política de senhas, todas as configurações de cookie do aplicativo podem ser alteradas no `Startup` classe.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Em `ConfigureServices` no `Startup` classe, você pode configurar o cookie do aplicativo.

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

---

`CookieAuthenticationOptions`tem as seguintes propriedades:

| Propriedade                | Descrição                       | Padrão |
| ----------------------- | --------------------------------- | ------- |
| `Cookie.Name`  | O nome do cookie.  | .AspNetCore.Cookies.  |
| `Cookie.HttpOnly`  | Quando for verdadeiro, o cookie não é acessível a partir de scripts do lado do cliente.  |  true |
| `ExpireTimeSpan`  | Controla o tempo que o tíquete de autenticação é armazenado no cookie permanecerá válido do ponto em que ele é criado.  | 14 dias  |
| `LoginPath`  | Quando um usuário está autorizado, ele será redirecionado para esse caminho para fazer logon. | / / Logon da conta  |
| `LogoutPath`  | Quando um usuário é desconectado, ele será redirecionado para esse caminho.  | / Conta/Logout  |
| `AccessDeniedPath`  | Quando um usuário executar uma verificação de autorização, ele será redirecionado para esse caminho.  |   |
| `SlidingExpiration`  | Quando for verdadeiro, será emitido um novo cookie com um novo tempo de expiração quando o cookie atual é mais de meio a janela de expiração.  | /Account/AccessDenied |
| `ReturnUrlParameter`  | Determina o nome do parâmetro de cadeia de caracteres de consulta que é acrescentado pelo middleware quando um código de 401 status não autorizado é alterado para um redirecionamento 302 para o caminho de logon.  |  true |
| `AuthenticationScheme`  | Isso só é relevante para o ASP.NET Core 1. x. O nome lógico para um esquema de autenticação específico. |  |
| `AutomaticAuthenticate`  | Esse sinalizador só é relevante para o ASP.NET Core 1. x. Quando for verdadeiro, autenticação de cookie deve executar em cada solicitação e tente validar e reconstrua qualquer entidade de segurança serializada criado por ele.  |  |
