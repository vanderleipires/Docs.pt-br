---
title: Configurar a identidade do ASP.NET Core
author: AdrienTorris
description: "Entender os valores padrão de identidade do ASP.NET Core e configure as várias propriedades de identidade para usar valores personalizados."
manager: wpickett
ms.author: scaddie
ms.date: 01/11/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/identity-configuration
ms.openlocfilehash: cf7dcdb80f5edf9e10960cb08957793c36829a69
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2018
---
# <a name="configure-identity"></a><span data-ttu-id="7a5c5-103">Configurar identidade</span><span class="sxs-lookup"><span data-stu-id="7a5c5-103">Configure Identity</span></span>

<span data-ttu-id="7a5c5-104">A identidade do ASP.NET Core tem comportamentos comuns em aplicativos, como a política de senha, tempo de bloqueio e configurações de cookie que você pode substituir facilmente em seu aplicativo `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="7a5c5-104">ASP.NET Core Identity has common behaviors in applications such as password policy, lockout time, and cookie settings that you can override easily in your application's `Startup` class.</span></span>

## <a name="passwords-policy"></a><span data-ttu-id="7a5c5-105">Política de senhas</span><span class="sxs-lookup"><span data-stu-id="7a5c5-105">Passwords policy</span></span>

<span data-ttu-id="7a5c5-106">Por padrão, a identidade requer que as senhas conter um caractere maiusculo, caractere minúsculo, um dígito e um caractere não alfanumérico.</span><span class="sxs-lookup"><span data-stu-id="7a5c5-106">By default, Identity requires that passwords contain an uppercase character, lowercase character, a digit, and a non-alphanumeric character.</span></span> <span data-ttu-id="7a5c5-107">Também há algumas outras restrições.</span><span class="sxs-lookup"><span data-stu-id="7a5c5-107">There are also some other restrictions.</span></span> <span data-ttu-id="7a5c5-108">Para simplificar as restrições de senha, modifique o `ConfigureServices` método o `Startup` classe do seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="7a5c5-108">To simplify password restrictions, modify the `ConfigureServices` method of the `Startup` class of your application.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7a5c5-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7a5c5-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="7a5c5-110">ASP.NET Core 2.0 adicionado o `RequiredUniqueChars` propriedade.</span><span class="sxs-lookup"><span data-stu-id="7a5c5-110">ASP.NET Core 2.0 added the `RequiredUniqueChars` property.</span></span> <span data-ttu-id="7a5c5-111">Caso contrário, as opções são as mesmas do ASP.NET Core 1. x.</span><span class="sxs-lookup"><span data-stu-id="7a5c5-111">Otherwise, the options are the same from ASP.NET Core 1.x.</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7a5c5-112">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7a5c5-112">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

---

<span data-ttu-id="7a5c5-113">`IdentityOptions.Password`tem as seguintes propriedades:</span><span class="sxs-lookup"><span data-stu-id="7a5c5-113">`IdentityOptions.Password` has the following properties:</span></span>

| <span data-ttu-id="7a5c5-114">Propriedade</span><span class="sxs-lookup"><span data-stu-id="7a5c5-114">Property</span></span>                | <span data-ttu-id="7a5c5-115">Descrição</span><span class="sxs-lookup"><span data-stu-id="7a5c5-115">Description</span></span>                       | <span data-ttu-id="7a5c5-116">Padrão</span><span class="sxs-lookup"><span data-stu-id="7a5c5-116">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `RequireDigit`          | <span data-ttu-id="7a5c5-117">Requer um número entre 0-9, a senha.</span><span class="sxs-lookup"><span data-stu-id="7a5c5-117">Requires a number between 0-9 in the password.</span></span> | <span data-ttu-id="7a5c5-118">true</span><span class="sxs-lookup"><span data-stu-id="7a5c5-118">true</span></span> |
| `RequiredLength`        | <span data-ttu-id="7a5c5-119">O comprimento mínimo da senha.</span><span class="sxs-lookup"><span data-stu-id="7a5c5-119">The minimum length of the password.</span></span> | <span data-ttu-id="7a5c5-120">6</span><span class="sxs-lookup"><span data-stu-id="7a5c5-120">6</span></span> |
| `RequireNonAlphanumeric`| <span data-ttu-id="7a5c5-121">Exige um caractere não alfanumérico na senha.</span><span class="sxs-lookup"><span data-stu-id="7a5c5-121">Requires a non-alphanumeric character in the password.</span></span> | <span data-ttu-id="7a5c5-122">true</span><span class="sxs-lookup"><span data-stu-id="7a5c5-122">true</span></span> |
| `RequireUppercase`      | <span data-ttu-id="7a5c5-123">Requer um caractere em letra maiuscula na senha.</span><span class="sxs-lookup"><span data-stu-id="7a5c5-123">Requires an upper case character in the password.</span></span> | <span data-ttu-id="7a5c5-124">true</span><span class="sxs-lookup"><span data-stu-id="7a5c5-124">true</span></span> |
| `RequireLowercase`      | <span data-ttu-id="7a5c5-125">Requer um caractere minúsculo na senha.</span><span class="sxs-lookup"><span data-stu-id="7a5c5-125">Requires a lower case character in the password.</span></span> | <span data-ttu-id="7a5c5-126">true</span><span class="sxs-lookup"><span data-stu-id="7a5c5-126">true</span></span> |
| `RequiredUniqueChars`   | <span data-ttu-id="7a5c5-127">Requer o número de caracteres distintos na senha.</span><span class="sxs-lookup"><span data-stu-id="7a5c5-127">Requires the number of distinct characters in the password.</span></span> | <span data-ttu-id="7a5c5-128">1</span><span class="sxs-lookup"><span data-stu-id="7a5c5-128">1</span></span> |


## <a name="users-lockout"></a><span data-ttu-id="7a5c5-129">Bloqueio do usuário</span><span class="sxs-lookup"><span data-stu-id="7a5c5-129">User's lockout</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,39-42,50-52)]

<span data-ttu-id="7a5c5-130">`IdentityOptions.Lockout`tem as seguintes propriedades:</span><span class="sxs-lookup"><span data-stu-id="7a5c5-130">`IdentityOptions.Lockout` has the following properties:</span></span>

| <span data-ttu-id="7a5c5-131">Propriedade</span><span class="sxs-lookup"><span data-stu-id="7a5c5-131">Property</span></span>                | <span data-ttu-id="7a5c5-132">Descrição</span><span class="sxs-lookup"><span data-stu-id="7a5c5-132">Description</span></span>                       | <span data-ttu-id="7a5c5-133">Padrão</span><span class="sxs-lookup"><span data-stu-id="7a5c5-133">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `DefaultLockoutTimeSpan` | <span data-ttu-id="7a5c5-134">A quantidade de tempo de uma usuário é bloqueada quando ocorre um bloqueio.</span><span class="sxs-lookup"><span data-stu-id="7a5c5-134">The amount of time a user is locked out when a lockout occurs.</span></span>  | <span data-ttu-id="7a5c5-135">5 minutos</span><span class="sxs-lookup"><span data-stu-id="7a5c5-135">5 minutes</span></span>  |
| `MaxFailedAccessAttempts` | <span data-ttu-id="7a5c5-136">O número de tentativas de acesso com falha até que um usuário seja bloqueado, se o bloqueio é ativado.</span><span class="sxs-lookup"><span data-stu-id="7a5c5-136">The number of failed access attempts until a user is locked out, if lockout is enabled.</span></span>  | <span data-ttu-id="7a5c5-137">5</span><span class="sxs-lookup"><span data-stu-id="7a5c5-137">5</span></span> |
| `AllowedForNewUsers` | <span data-ttu-id="7a5c5-138">Determina se um novo usuário poderá ser bloqueado.</span><span class="sxs-lookup"><span data-stu-id="7a5c5-138">Determines if a new user can be locked out.</span></span>  | <span data-ttu-id="7a5c5-139">true</span><span class="sxs-lookup"><span data-stu-id="7a5c5-139">true</span></span> |

## <a name="sign-in-settings"></a><span data-ttu-id="7a5c5-140">Configurações de entrada</span><span class="sxs-lookup"><span data-stu-id="7a5c5-140">Sign in settings</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)]

<span data-ttu-id="7a5c5-141">`IdentityOptions.SignIn`tem as seguintes propriedades:</span><span class="sxs-lookup"><span data-stu-id="7a5c5-141">`IdentityOptions.SignIn` has the following properties:</span></span>

| <span data-ttu-id="7a5c5-142">Propriedade</span><span class="sxs-lookup"><span data-stu-id="7a5c5-142">Property</span></span>                | <span data-ttu-id="7a5c5-143">Descrição</span><span class="sxs-lookup"><span data-stu-id="7a5c5-143">Description</span></span>                       | <span data-ttu-id="7a5c5-144">Padrão</span><span class="sxs-lookup"><span data-stu-id="7a5c5-144">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `RequireConfirmedEmail` | <span data-ttu-id="7a5c5-145">Requer um email confirmado para entrar.</span><span class="sxs-lookup"><span data-stu-id="7a5c5-145">Requires a confirmed email to sign in.</span></span> | <span data-ttu-id="7a5c5-146">false</span><span class="sxs-lookup"><span data-stu-id="7a5c5-146">false</span></span>  |
| `RequireConfirmedPhoneNumber` |  <span data-ttu-id="7a5c5-147">Requer um número de telefone confirmado entrar.</span><span class="sxs-lookup"><span data-stu-id="7a5c5-147">Requires a confirmed phone number to sign in.</span></span> | <span data-ttu-id="7a5c5-148">false</span><span class="sxs-lookup"><span data-stu-id="7a5c5-148">false</span></span>  |

## <a name="user-validation-settings"></a><span data-ttu-id="7a5c5-149">Configurações de validação do usuário</span><span class="sxs-lookup"><span data-stu-id="7a5c5-149">User validation settings</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,48-52)]

<span data-ttu-id="7a5c5-150">`IdentityOptions.User`tem as seguintes propriedades:</span><span class="sxs-lookup"><span data-stu-id="7a5c5-150">`IdentityOptions.User` has the following properties:</span></span>

| <span data-ttu-id="7a5c5-151">Propriedade</span><span class="sxs-lookup"><span data-stu-id="7a5c5-151">Property</span></span>                | <span data-ttu-id="7a5c5-152">Descrição</span><span class="sxs-lookup"><span data-stu-id="7a5c5-152">Description</span></span>                       | <span data-ttu-id="7a5c5-153">Padrão</span><span class="sxs-lookup"><span data-stu-id="7a5c5-153">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `RequireUniqueEmail`  | <span data-ttu-id="7a5c5-154">Requer que cada usuário tenha um email exclusivo.</span><span class="sxs-lookup"><span data-stu-id="7a5c5-154">Requires each User to have a unique email.</span></span> | <span data-ttu-id="7a5c5-155">false</span><span class="sxs-lookup"><span data-stu-id="7a5c5-155">false</span></span>  |
| `AllowedUserNameCharacters`  | <span data-ttu-id="7a5c5-156">Caracteres permitidos no nome do usuário.</span><span class="sxs-lookup"><span data-stu-id="7a5c5-156">Allowed characters in the username.</span></span> | <span data-ttu-id="7a5c5-157">abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+</span><span class="sxs-lookup"><span data-stu-id="7a5c5-157">abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+</span></span> |


## <a name="applications-cookie-settings"></a><span data-ttu-id="7a5c5-158">Configurações de cookie do aplicativo</span><span class="sxs-lookup"><span data-stu-id="7a5c5-158">Application's cookie settings</span></span>

<span data-ttu-id="7a5c5-159">Como a política de senhas, todas as configurações de cookie do aplicativo podem ser alteradas no `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="7a5c5-159">Like the passwords policy, all the settings of the application's cookie can be changed in the `Startup` class.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7a5c5-160">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7a5c5-160">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="7a5c5-161">Em `ConfigureServices` no `Startup` classe, você pode configurar o cookie do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="7a5c5-161">Under `ConfigureServices` in the `Startup` class, you can configure the application's cookie.</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7a5c5-162">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7a5c5-162">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

---

<span data-ttu-id="7a5c5-163">`CookieAuthenticationOptions`tem as seguintes propriedades:</span><span class="sxs-lookup"><span data-stu-id="7a5c5-163">`CookieAuthenticationOptions` has the following properties:</span></span>

| <span data-ttu-id="7a5c5-164">Propriedade</span><span class="sxs-lookup"><span data-stu-id="7a5c5-164">Property</span></span>                | <span data-ttu-id="7a5c5-165">Descrição</span><span class="sxs-lookup"><span data-stu-id="7a5c5-165">Description</span></span>                       | <span data-ttu-id="7a5c5-166">Padrão</span><span class="sxs-lookup"><span data-stu-id="7a5c5-166">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `Cookie.Name`  | <span data-ttu-id="7a5c5-167">O nome do cookie.</span><span class="sxs-lookup"><span data-stu-id="7a5c5-167">The name of the cookie.</span></span>  | <span data-ttu-id="7a5c5-168">.AspNetCore.Cookies.</span><span class="sxs-lookup"><span data-stu-id="7a5c5-168">.AspNetCore.Cookies.</span></span>  |
| `Cookie.HttpOnly`  | <span data-ttu-id="7a5c5-169">Quando for verdadeiro, o cookie não é acessível a partir de scripts do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="7a5c5-169">When true, the cookie isn't accessible from client-side scripts.</span></span>  |  <span data-ttu-id="7a5c5-170">true</span><span class="sxs-lookup"><span data-stu-id="7a5c5-170">true</span></span> |
| `ExpireTimeSpan`  | <span data-ttu-id="7a5c5-171">Controla o tempo que o tíquete de autenticação é armazenado no cookie permanecerá válido do ponto em que ele é criado.</span><span class="sxs-lookup"><span data-stu-id="7a5c5-171">Controls how much time the authentication ticket stored in the cookie will remain valid from the point it's created.</span></span>  | <span data-ttu-id="7a5c5-172">14 dias</span><span class="sxs-lookup"><span data-stu-id="7a5c5-172">14 days</span></span>  |
| `LoginPath`  | <span data-ttu-id="7a5c5-173">Quando um usuário está autorizado, ele será redirecionado para esse caminho para fazer logon.</span><span class="sxs-lookup"><span data-stu-id="7a5c5-173">When a user is unauthorized, they will be redirected to this path to login.</span></span> | <span data-ttu-id="7a5c5-174">/ / Logon da conta</span><span class="sxs-lookup"><span data-stu-id="7a5c5-174">/Account/Login</span></span>  |
| `LogoutPath`  | <span data-ttu-id="7a5c5-175">Quando um usuário é desconectado, ele será redirecionado para esse caminho.</span><span class="sxs-lookup"><span data-stu-id="7a5c5-175">When a user is logged out, they will be redirected to this path.</span></span>  | <span data-ttu-id="7a5c5-176">/ Conta/Logout</span><span class="sxs-lookup"><span data-stu-id="7a5c5-176">/Account/Logout</span></span>  |
| `AccessDeniedPath`  | <span data-ttu-id="7a5c5-177">Quando um usuário executar uma verificação de autorização, ele será redirecionado para esse caminho.</span><span class="sxs-lookup"><span data-stu-id="7a5c5-177">When a user fails an authorization check, they will be redirected to this path.</span></span>  |   |
| `SlidingExpiration`  | <span data-ttu-id="7a5c5-178">Quando for verdadeiro, será emitido um novo cookie com um novo tempo de expiração quando o cookie atual é mais de meio a janela de expiração.</span><span class="sxs-lookup"><span data-stu-id="7a5c5-178">When true, a new cookie will be issued with a new expiration time when the current cookie is more than halfway through the expiration window.</span></span>  | <span data-ttu-id="7a5c5-179">/Account/AccessDenied</span><span class="sxs-lookup"><span data-stu-id="7a5c5-179">/Account/AccessDenied</span></span> |
| `ReturnUrlParameter`  | <span data-ttu-id="7a5c5-180">Determina o nome do parâmetro de cadeia de caracteres de consulta que é acrescentado pelo middleware quando um código de 401 status não autorizado é alterado para um redirecionamento 302 para o caminho de logon.</span><span class="sxs-lookup"><span data-stu-id="7a5c5-180">Determines the name of the query string parameter which is appended by the middleware when a 401 Unauthorized status code is changed to a 302 redirect onto the login path.</span></span>  |  <span data-ttu-id="7a5c5-181">true</span><span class="sxs-lookup"><span data-stu-id="7a5c5-181">true</span></span> |
| `AuthenticationScheme`  | <span data-ttu-id="7a5c5-182">Isso só é relevante para o ASP.NET Core 1. x.</span><span class="sxs-lookup"><span data-stu-id="7a5c5-182">This is only relevant for ASP.NET Core 1.x.</span></span> <span data-ttu-id="7a5c5-183">O nome lógico para um esquema de autenticação específico.</span><span class="sxs-lookup"><span data-stu-id="7a5c5-183">The logical name for a particular authentication scheme.</span></span> |  |
| `AutomaticAuthenticate`  | <span data-ttu-id="7a5c5-184">Esse sinalizador só é relevante para o ASP.NET Core 1. x.</span><span class="sxs-lookup"><span data-stu-id="7a5c5-184">This flag is only relevant for ASP.NET Core 1.x.</span></span> <span data-ttu-id="7a5c5-185">Quando for verdadeiro, autenticação de cookie deve executar em cada solicitação e tente validar e reconstrua qualquer entidade de segurança serializada criado por ele.</span><span class="sxs-lookup"><span data-stu-id="7a5c5-185">When true, cookie authentication should run on every request and attempt to validate and reconstruct any serialized principal it created.</span></span>  |  |
