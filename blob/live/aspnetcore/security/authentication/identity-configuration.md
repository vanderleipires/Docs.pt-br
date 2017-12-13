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
ms.openlocfilehash: 2861ca474e7e82da81943966394a92040ce96ab8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
# <a name="configure-identity"></a><span data-ttu-id="01687-104">Configurar identidade</span><span class="sxs-lookup"><span data-stu-id="01687-104">Configure Identity</span></span>

<span data-ttu-id="01687-105">A identidade do ASP.NET Core tem alguns comportamentos padrão que você pode substituir facilmente em seu aplicativo `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="01687-105">ASP.NET Core Identity has some default behaviors that you can override easily in your application's `Startup` class.</span></span>

## <a name="passwords-policy"></a><span data-ttu-id="01687-106">Política de senhas</span><span class="sxs-lookup"><span data-stu-id="01687-106">Passwords policy</span></span>

<span data-ttu-id="01687-107">Por padrão, a identidade requer que as senhas conter um caractere maiusculo, caractere minúsculo, um dígito e um caractere não alfanumérico.</span><span class="sxs-lookup"><span data-stu-id="01687-107">By default, Identity requires that passwords contain an uppercase character, lowercase character, a digit, and a non-alphanumeric character.</span></span> <span data-ttu-id="01687-108">Também há algumas outras restrições.</span><span class="sxs-lookup"><span data-stu-id="01687-108">There are also some other restrictions.</span></span> <span data-ttu-id="01687-109">Se você deseja simplificar a restrições de senha, você pode fazer isso no `Startup` classe do seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="01687-109">If you want to simplify password restrictions, you can do that in the `Startup` class of your application.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="01687-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="01687-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="01687-111">ASP.NET Core 2.0 adicionado o `RequiredUniqueChars` propriedade.</span><span class="sxs-lookup"><span data-stu-id="01687-111">ASP.NET Core 2.0 added the `RequiredUniqueChars` property.</span></span> <span data-ttu-id="01687-112">Caso contrário, as opções são as mesmas do ASP.NET Core 1. x.</span><span class="sxs-lookup"><span data-stu-id="01687-112">Otherwise, the options are the same from ASP.NET Core 1.x.</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="01687-113">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="01687-113">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

---

<span data-ttu-id="01687-114">`IdentityOptions.Password`tem as seguintes propriedades:</span><span class="sxs-lookup"><span data-stu-id="01687-114">`IdentityOptions.Password` has the following properties:</span></span>
* <span data-ttu-id="01687-115">`RequireDigit`: Exige um número entre 0-9, a senha.</span><span class="sxs-lookup"><span data-stu-id="01687-115">`RequireDigit`: Requires a number between 0-9 in the password.</span></span> <span data-ttu-id="01687-116">Padrão é true.</span><span class="sxs-lookup"><span data-stu-id="01687-116">Defaults to true.</span></span>
* <span data-ttu-id="01687-117">`RequiredLength`: O comprimento mínimo da senha.</span><span class="sxs-lookup"><span data-stu-id="01687-117">`RequiredLength`: The minimum length of the password.</span></span> <span data-ttu-id="01687-118">O padrão é 6.</span><span class="sxs-lookup"><span data-stu-id="01687-118">Defaults to 6.</span></span>
* <span data-ttu-id="01687-119">`RequireNonAlphanumeric`: Requer um caractere não alfanumérico na senha.</span><span class="sxs-lookup"><span data-stu-id="01687-119">`RequireNonAlphanumeric`: Requires a non-alphanumeric character in the password.</span></span> <span data-ttu-id="01687-120">Padrão é true.</span><span class="sxs-lookup"><span data-stu-id="01687-120">Defaults to true.</span></span>
* <span data-ttu-id="01687-121">`RequireUppercase`: Requer um caractere em letra maiuscula na senha.</span><span class="sxs-lookup"><span data-stu-id="01687-121">`RequireUppercase`: Requires an upper case character in the password.</span></span> <span data-ttu-id="01687-122">Padrão é true.</span><span class="sxs-lookup"><span data-stu-id="01687-122">Defaults to true.</span></span>
* <span data-ttu-id="01687-123">`RequireLowercase`: Requer um caractere minúsculo na senha.</span><span class="sxs-lookup"><span data-stu-id="01687-123">`RequireLowercase`: Requires a lower case character in the password.</span></span> <span data-ttu-id="01687-124">Padrão é true.</span><span class="sxs-lookup"><span data-stu-id="01687-124">Defaults to true.</span></span>
* <span data-ttu-id="01687-125">`RequiredUniqueChars`: Requer o número de caracteres distintos na senha.</span><span class="sxs-lookup"><span data-stu-id="01687-125">`RequiredUniqueChars`: Requires the number of distinct characters in the password.</span></span> <span data-ttu-id="01687-126">O padrão é 1.</span><span class="sxs-lookup"><span data-stu-id="01687-126">Defaults to 1.</span></span>


## <a name="users-lockout"></a><span data-ttu-id="01687-127">Bloqueio do usuário</span><span class="sxs-lookup"><span data-stu-id="01687-127">User's lockout</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,39-42,50-52)]

<span data-ttu-id="01687-128">`IdentityOptions.Lockout`tem as seguintes propriedades:</span><span class="sxs-lookup"><span data-stu-id="01687-128">`IdentityOptions.Lockout` has the following properties:</span></span>
* <span data-ttu-id="01687-129">`DefaultLockoutTimeSpan`: A quantidade de tempo de uma usuário é bloqueada quando ocorre um bloqueio.</span><span class="sxs-lookup"><span data-stu-id="01687-129">`DefaultLockoutTimeSpan`: The amount of time a user is locked out when a lockout occurs.</span></span> <span data-ttu-id="01687-130">O padrão é 5 minutos.</span><span class="sxs-lookup"><span data-stu-id="01687-130">Defaults to 5 minutes.</span></span>
* <span data-ttu-id="01687-131">`MaxFailedAccessAttempts`: O número de tentativas de acesso com falha até que um usuário seja bloqueado, se o bloqueio é ativado.</span><span class="sxs-lookup"><span data-stu-id="01687-131">`MaxFailedAccessAttempts`: The number of failed access attempts until a user is locked out, if lockout is enabled.</span></span> <span data-ttu-id="01687-132">O padrão é 5.</span><span class="sxs-lookup"><span data-stu-id="01687-132">Defaults to 5.</span></span>
* <span data-ttu-id="01687-133">`AllowedForNewUsers`: Determina se um novo usuário poderá ser bloqueado. Padrão é true.</span><span class="sxs-lookup"><span data-stu-id="01687-133">`AllowedForNewUsers`: Determines if a new user can be locked out. Defaults to true.</span></span>


## <a name="sign-in-settings"></a><span data-ttu-id="01687-134">Configurações de entrada</span><span class="sxs-lookup"><span data-stu-id="01687-134">Sign in settings</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)]

<span data-ttu-id="01687-135">`IdentityOptions.SignIn`tem as seguintes propriedades:</span><span class="sxs-lookup"><span data-stu-id="01687-135">`IdentityOptions.SignIn` has the following properties:</span></span>
* <span data-ttu-id="01687-136">`RequireConfirmedEmail`: Requer um email confirmado para entrar.</span><span class="sxs-lookup"><span data-stu-id="01687-136">`RequireConfirmedEmail`: Requires a confirmed email to sign in.</span></span> <span data-ttu-id="01687-137">O padrão é false.</span><span class="sxs-lookup"><span data-stu-id="01687-137">Defaults to false.</span></span>
* <span data-ttu-id="01687-138">`RequireConfirmedPhoneNumber`: Requer um número de telefone confirmado entrar.</span><span class="sxs-lookup"><span data-stu-id="01687-138">`RequireConfirmedPhoneNumber`: Requires a confirmed phone number to sign in.</span></span> <span data-ttu-id="01687-139">O padrão é false.</span><span class="sxs-lookup"><span data-stu-id="01687-139">Defaults to false.</span></span>


## <a name="user-validation-settings"></a><span data-ttu-id="01687-140">Configurações de validação do usuário</span><span class="sxs-lookup"><span data-stu-id="01687-140">User validation settings</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,48-52)]

<span data-ttu-id="01687-141">`IdentityOptions.User`tem as seguintes propriedades:</span><span class="sxs-lookup"><span data-stu-id="01687-141">`IdentityOptions.User` has the following properties:</span></span>
* <span data-ttu-id="01687-142">`RequireUniqueEmail`: Exige que cada usuário tenha um email exclusivo.</span><span class="sxs-lookup"><span data-stu-id="01687-142">`RequireUniqueEmail`: Requires each User to have a unique email.</span></span> <span data-ttu-id="01687-143">O padrão é false.</span><span class="sxs-lookup"><span data-stu-id="01687-143">Defaults to false.</span></span>
* <span data-ttu-id="01687-144">`AllowedUserNameCharacters`: Caracteres permitidos no nome do usuário.</span><span class="sxs-lookup"><span data-stu-id="01687-144">`AllowedUserNameCharacters`: Allowed characters in the username.</span></span> <span data-ttu-id="01687-145">O padrão é abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+.</span><span class="sxs-lookup"><span data-stu-id="01687-145">Defaults to abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+.</span></span>

## <a name="applications-cookie-settings"></a><span data-ttu-id="01687-146">Configurações de cookie do aplicativo</span><span class="sxs-lookup"><span data-stu-id="01687-146">Application's cookie settings</span></span>

<span data-ttu-id="01687-147">Como a política de senhas, todas as configurações de cookie do aplicativo podem ser alteradas no `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="01687-147">Like the passwords policy, all the settings of the application's cookie can be changed in the `Startup` class.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="01687-148">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="01687-148">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="01687-149">Em `ConfigureServices` no `Startup` classe, você pode configurar o cookie do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="01687-149">Under `ConfigureServices` in the `Startup` class, you can configure the application's cookie.</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="01687-150">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="01687-150">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

--- 

<span data-ttu-id="01687-151">`CookieAuthenticationOptions`tem as seguintes propriedades:</span><span class="sxs-lookup"><span data-stu-id="01687-151">`CookieAuthenticationOptions` has the following properties:</span></span>
* <span data-ttu-id="01687-152">`Cookie.Name`: O nome do cookie.</span><span class="sxs-lookup"><span data-stu-id="01687-152">`Cookie.Name`: The name of the cookie.</span></span> <span data-ttu-id="01687-153">O padrão é. AspNetCore.Cookies.</span><span class="sxs-lookup"><span data-stu-id="01687-153">Defaults to .AspNetCore.Cookies.</span></span>
* <span data-ttu-id="01687-154">`Cookie.HttpOnly`: Quando for verdadeiro, o cookie não é acessível a partir de scripts do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="01687-154">`Cookie.HttpOnly`: When true, the cookie is not accessible from client-side scripts.</span></span> <span data-ttu-id="01687-155">Padrão é true.</span><span class="sxs-lookup"><span data-stu-id="01687-155">Defaults to true.</span></span>
* <span data-ttu-id="01687-156">`ExpireTimeSpan`: Controla o tempo que o tíquete de autenticação é armazenado no cookie permanecerá válido do ponto em que ele é criado.</span><span class="sxs-lookup"><span data-stu-id="01687-156">`ExpireTimeSpan`: Controls how much time the authentication ticket stored in the cookie will remain valid from the point it is created.</span></span> <span data-ttu-id="01687-157">O padrão é 14 dias.</span><span class="sxs-lookup"><span data-stu-id="01687-157">Defaults to 14 days.</span></span>
* <span data-ttu-id="01687-158">`LoginPath`: Quando um usuário está autorizado, ele será redirecionado para esse caminho para fazer logon.</span><span class="sxs-lookup"><span data-stu-id="01687-158">`LoginPath`: When a user is unauthorized, they will be redirected to this path to login.</span></span> <span data-ttu-id="01687-159">O padrão é a conta/logon.</span><span class="sxs-lookup"><span data-stu-id="01687-159">Defaults to /Account/Login.</span></span>
* <span data-ttu-id="01687-160">`LogoutPath`: Quando um usuário é desconectado, ele será redirecionado para esse caminho.</span><span class="sxs-lookup"><span data-stu-id="01687-160">`LogoutPath`: When a user is logged out, they will be redirected to this path.</span></span> <span data-ttu-id="01687-161">O padrão é a conta/logoff.</span><span class="sxs-lookup"><span data-stu-id="01687-161">Defaults to /Account/Logout.</span></span>
* <span data-ttu-id="01687-162">`AccessDeniedPath`: Quando um usuário não é uma verificação de autorização, ele será redirecionado para esse caminho.</span><span class="sxs-lookup"><span data-stu-id="01687-162">`AccessDeniedPath`: When a user fails an authorization check, they will be redirected to this path.</span></span> <span data-ttu-id="01687-163">O padrão é a conta/AccessDenied.</span><span class="sxs-lookup"><span data-stu-id="01687-163">Defaults to /Account/AccessDenied.</span></span>
* <span data-ttu-id="01687-164">`SlidingExpiration`: Quando for verdadeiro, será emitido um novo cookie com um novo tempo de expiração quando o cookie atual é a janela de validade máximo na metade.</span><span class="sxs-lookup"><span data-stu-id="01687-164">`SlidingExpiration`: When true, a new cookie will be issued with a new expiration time when the current cookie is more than halfway through the expiration window.</span></span> <span data-ttu-id="01687-165">Padrão é true.</span><span class="sxs-lookup"><span data-stu-id="01687-165">Defaults to true.</span></span>
* <span data-ttu-id="01687-166">`ReturnUrlParameter`: O ReturnUrlParameter determina o nome do parâmetro de cadeia de caracteres de consulta que é acrescentado pelo middleware quando um código de 401 status não autorizado é alterado para um redirecionamento 302 para o caminho de logon.</span><span class="sxs-lookup"><span data-stu-id="01687-166">`ReturnUrlParameter`: The ReturnUrlParameter determines the name of the query string parameter which is appended by the middleware when a 401 Unauthorized status code is changed to a 302 redirect onto the login path.</span></span>
* <span data-ttu-id="01687-167">`AuthenticationScheme`: Isso só é relevante para o ASP.NET Core 1. x.</span><span class="sxs-lookup"><span data-stu-id="01687-167">`AuthenticationScheme`: This is only relevant for ASP.NET Core 1.x.</span></span> <span data-ttu-id="01687-168">O nome lógico para um esquema de autenticação específico.</span><span class="sxs-lookup"><span data-stu-id="01687-168">The logical name for a particular authentication scheme.</span></span>
* <span data-ttu-id="01687-169">`AutomaticAuthenticate`: Esse sinalizador só é relevante para o ASP.NET Core 1. x.</span><span class="sxs-lookup"><span data-stu-id="01687-169">`AutomaticAuthenticate`: This flag is only relevant for ASP.NET Core 1.x.</span></span> <span data-ttu-id="01687-170">Quando for verdadeiro, autenticação de cookie deve executar em cada solicitação e tente validar e reconstrua qualquer entidade de segurança serializada criado por ele.</span><span class="sxs-lookup"><span data-stu-id="01687-170">When true, cookie authentication should run on every request and attempt to validate and reconstruct any serialized principal it created.</span></span>

