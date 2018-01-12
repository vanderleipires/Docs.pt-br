---
title: Configurar a identidade do ASP.NET Core
author: AdrienTorris
description: "Entender os valores padrão de identidade do ASP.NET Core e configure as várias propriedades de identidade para usar valores personalizados."
keywords: "Autenticação do ASP.NET Core, identidade, segurança"
ms.author: scaddie
manager: wpickett
ms.date: 01/11/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity-configuration
ms.openlocfilehash: ac204cb89aac1f90adc64c4f0bec4e946cb8c4d9
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/11/2018
---
# <a name="configure-identity"></a><span data-ttu-id="a5dc2-104">Configurar identidade</span><span class="sxs-lookup"><span data-stu-id="a5dc2-104">Configure Identity</span></span>

<span data-ttu-id="a5dc2-105">A identidade do ASP.NET Core tem comportamentos comuns em aplicativos, como a política de senha, tempo de bloqueio e configurações de cookie que você pode substituir facilmente em seu aplicativo `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="a5dc2-105">ASP.NET Core Identity has common behaviors in applications such as password policy, lockout time, and cookie settings that you can override easily in your application's `Startup` class.</span></span>

## <a name="passwords-policy"></a><span data-ttu-id="a5dc2-106">Política de senhas</span><span class="sxs-lookup"><span data-stu-id="a5dc2-106">Passwords policy</span></span>

<span data-ttu-id="a5dc2-107">Por padrão, a identidade requer que as senhas conter um caractere maiusculo, caractere minúsculo, um dígito e um caractere não alfanumérico.</span><span class="sxs-lookup"><span data-stu-id="a5dc2-107">By default, Identity requires that passwords contain an uppercase character, lowercase character, a digit, and a non-alphanumeric character.</span></span> <span data-ttu-id="a5dc2-108">Também há algumas outras restrições.</span><span class="sxs-lookup"><span data-stu-id="a5dc2-108">There are also some other restrictions.</span></span> <span data-ttu-id="a5dc2-109">Para simplificar as restrições de senha, modifique o `ConfigureServices` método o `Startup` classe do seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="a5dc2-109">To simplify password restrictions, modify the `ConfigureServices` method of the `Startup` class of your application.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a5dc2-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a5dc2-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="a5dc2-111">ASP.NET Core 2.0 adicionado o `RequiredUniqueChars` propriedade.</span><span class="sxs-lookup"><span data-stu-id="a5dc2-111">ASP.NET Core 2.0 added the `RequiredUniqueChars` property.</span></span> <span data-ttu-id="a5dc2-112">Caso contrário, as opções são as mesmas do ASP.NET Core 1. x.</span><span class="sxs-lookup"><span data-stu-id="a5dc2-112">Otherwise, the options are the same from ASP.NET Core 1.x.</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a5dc2-113">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a5dc2-113">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

---

<span data-ttu-id="a5dc2-114">`IdentityOptions.Password`tem as seguintes propriedades:</span><span class="sxs-lookup"><span data-stu-id="a5dc2-114">`IdentityOptions.Password` has the following properties:</span></span>

| <span data-ttu-id="a5dc2-115">Propriedade</span><span class="sxs-lookup"><span data-stu-id="a5dc2-115">Property</span></span>                | <span data-ttu-id="a5dc2-116">Descrição</span><span class="sxs-lookup"><span data-stu-id="a5dc2-116">Description</span></span>                       | <span data-ttu-id="a5dc2-117">Padrão</span><span class="sxs-lookup"><span data-stu-id="a5dc2-117">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `RequireDigit`          | <span data-ttu-id="a5dc2-118">Requer um número entre 0-9, a senha.</span><span class="sxs-lookup"><span data-stu-id="a5dc2-118">Requires a number between 0-9 in the password.</span></span> | <span data-ttu-id="a5dc2-119">true</span><span class="sxs-lookup"><span data-stu-id="a5dc2-119">true</span></span> |
| `RequiredLength`        | <span data-ttu-id="a5dc2-120">O comprimento mínimo da senha.</span><span class="sxs-lookup"><span data-stu-id="a5dc2-120">The minimum length of the password.</span></span> | <span data-ttu-id="a5dc2-121">6</span><span class="sxs-lookup"><span data-stu-id="a5dc2-121">6</span></span> |
| `RequireNonAlphanumeric`| <span data-ttu-id="a5dc2-122">Exige um caractere não alfanumérico na senha.</span><span class="sxs-lookup"><span data-stu-id="a5dc2-122">Requires a non-alphanumeric character in the password.</span></span> | <span data-ttu-id="a5dc2-123">true</span><span class="sxs-lookup"><span data-stu-id="a5dc2-123">true</span></span> |
| `RequireUppercase`      | <span data-ttu-id="a5dc2-124">Requer um caractere em letra maiuscula na senha.</span><span class="sxs-lookup"><span data-stu-id="a5dc2-124">Requires an upper case character in the password.</span></span> | <span data-ttu-id="a5dc2-125">true</span><span class="sxs-lookup"><span data-stu-id="a5dc2-125">true</span></span> |
| `RequireLowercase`      | <span data-ttu-id="a5dc2-126">Requer um caractere minúsculo na senha.</span><span class="sxs-lookup"><span data-stu-id="a5dc2-126">Requires a lower case character in the password.</span></span> | <span data-ttu-id="a5dc2-127">true</span><span class="sxs-lookup"><span data-stu-id="a5dc2-127">true</span></span> |
| `RequiredUniqueChars`   | <span data-ttu-id="a5dc2-128">Requer o número de caracteres distintos na senha.</span><span class="sxs-lookup"><span data-stu-id="a5dc2-128">Requires the number of distinct characters in the password.</span></span> | <span data-ttu-id="a5dc2-129">1</span><span class="sxs-lookup"><span data-stu-id="a5dc2-129">1</span></span> |


## <a name="users-lockout"></a><span data-ttu-id="a5dc2-130">Bloqueio do usuário</span><span class="sxs-lookup"><span data-stu-id="a5dc2-130">User's lockout</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,39-42,50-52)]

<span data-ttu-id="a5dc2-131">`IdentityOptions.Lockout`tem as seguintes propriedades:</span><span class="sxs-lookup"><span data-stu-id="a5dc2-131">`IdentityOptions.Lockout` has the following properties:</span></span>

| <span data-ttu-id="a5dc2-132">Propriedade</span><span class="sxs-lookup"><span data-stu-id="a5dc2-132">Property</span></span>                | <span data-ttu-id="a5dc2-133">Descrição</span><span class="sxs-lookup"><span data-stu-id="a5dc2-133">Description</span></span>                       | <span data-ttu-id="a5dc2-134">Padrão</span><span class="sxs-lookup"><span data-stu-id="a5dc2-134">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `DefaultLockoutTimeSpan` | <span data-ttu-id="a5dc2-135">A quantidade de tempo de uma usuário é bloqueada quando ocorre um bloqueio.</span><span class="sxs-lookup"><span data-stu-id="a5dc2-135">The amount of time a user is locked out when a lockout occurs.</span></span>  | <span data-ttu-id="a5dc2-136">5 minutos</span><span class="sxs-lookup"><span data-stu-id="a5dc2-136">5 minutes</span></span>  |
| `MaxFailedAccessAttempts` | <span data-ttu-id="a5dc2-137">O número de tentativas de acesso com falha até que um usuário seja bloqueado, se o bloqueio é ativado.</span><span class="sxs-lookup"><span data-stu-id="a5dc2-137">The number of failed access attempts until a user is locked out, if lockout is enabled.</span></span>  | <span data-ttu-id="a5dc2-138">5</span><span class="sxs-lookup"><span data-stu-id="a5dc2-138">5</span></span> |
| `AllowedForNewUsers` | <span data-ttu-id="a5dc2-139">Determina se um novo usuário poderá ser bloqueado.</span><span class="sxs-lookup"><span data-stu-id="a5dc2-139">Determines if a new user can be locked out.</span></span>  | <span data-ttu-id="a5dc2-140">true</span><span class="sxs-lookup"><span data-stu-id="a5dc2-140">true</span></span> |

## <a name="sign-in-settings"></a><span data-ttu-id="a5dc2-141">Configurações de entrada</span><span class="sxs-lookup"><span data-stu-id="a5dc2-141">Sign in settings</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)]

<span data-ttu-id="a5dc2-142">`IdentityOptions.SignIn`tem as seguintes propriedades:</span><span class="sxs-lookup"><span data-stu-id="a5dc2-142">`IdentityOptions.SignIn` has the following properties:</span></span>

| <span data-ttu-id="a5dc2-143">Propriedade</span><span class="sxs-lookup"><span data-stu-id="a5dc2-143">Property</span></span>                | <span data-ttu-id="a5dc2-144">Descrição</span><span class="sxs-lookup"><span data-stu-id="a5dc2-144">Description</span></span>                       | <span data-ttu-id="a5dc2-145">Padrão</span><span class="sxs-lookup"><span data-stu-id="a5dc2-145">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `RequireConfirmedEmail` | <span data-ttu-id="a5dc2-146">Requer um email confirmado para entrar.</span><span class="sxs-lookup"><span data-stu-id="a5dc2-146">Requires a confirmed email to sign in.</span></span> | <span data-ttu-id="a5dc2-147">false</span><span class="sxs-lookup"><span data-stu-id="a5dc2-147">false</span></span>  |
| `RequireConfirmedPhoneNumber` |  <span data-ttu-id="a5dc2-148">Requer um número de telefone confirmado entrar.</span><span class="sxs-lookup"><span data-stu-id="a5dc2-148">Requires a confirmed phone number to sign in.</span></span> | <span data-ttu-id="a5dc2-149">false</span><span class="sxs-lookup"><span data-stu-id="a5dc2-149">false</span></span>  |

## <a name="user-validation-settings"></a><span data-ttu-id="a5dc2-150">Configurações de validação do usuário</span><span class="sxs-lookup"><span data-stu-id="a5dc2-150">User validation settings</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,48-52)]

<span data-ttu-id="a5dc2-151">`IdentityOptions.User`tem as seguintes propriedades:</span><span class="sxs-lookup"><span data-stu-id="a5dc2-151">`IdentityOptions.User` has the following properties:</span></span>

| <span data-ttu-id="a5dc2-152">Propriedade</span><span class="sxs-lookup"><span data-stu-id="a5dc2-152">Property</span></span>                | <span data-ttu-id="a5dc2-153">Descrição</span><span class="sxs-lookup"><span data-stu-id="a5dc2-153">Description</span></span>                       | <span data-ttu-id="a5dc2-154">Padrão</span><span class="sxs-lookup"><span data-stu-id="a5dc2-154">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `RequireUniqueEmail`  | <span data-ttu-id="a5dc2-155">Requer que cada usuário tenha um email exclusivo.</span><span class="sxs-lookup"><span data-stu-id="a5dc2-155">Requires each User to have a unique email.</span></span> | <span data-ttu-id="a5dc2-156">false</span><span class="sxs-lookup"><span data-stu-id="a5dc2-156">false</span></span>  |
| `AllowedUserNameCharacters`  | <span data-ttu-id="a5dc2-157">Caracteres permitidos no nome do usuário.</span><span class="sxs-lookup"><span data-stu-id="a5dc2-157">Allowed characters in the username.</span></span> | <span data-ttu-id="a5dc2-158">abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+</span><span class="sxs-lookup"><span data-stu-id="a5dc2-158">abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+</span></span> |


## <a name="applications-cookie-settings"></a><span data-ttu-id="a5dc2-159">Configurações de cookie do aplicativo</span><span class="sxs-lookup"><span data-stu-id="a5dc2-159">Application's cookie settings</span></span>

<span data-ttu-id="a5dc2-160">Como a política de senhas, todas as configurações de cookie do aplicativo podem ser alteradas no `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="a5dc2-160">Like the passwords policy, all the settings of the application's cookie can be changed in the `Startup` class.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a5dc2-161">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a5dc2-161">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="a5dc2-162">Em `ConfigureServices` no `Startup` classe, você pode configurar o cookie do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="a5dc2-162">Under `ConfigureServices` in the `Startup` class, you can configure the application's cookie.</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a5dc2-163">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a5dc2-163">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

---

<span data-ttu-id="a5dc2-164">`CookieAuthenticationOptions`tem as seguintes propriedades:</span><span class="sxs-lookup"><span data-stu-id="a5dc2-164">`CookieAuthenticationOptions` has the following properties:</span></span>

| <span data-ttu-id="a5dc2-165">Propriedade</span><span class="sxs-lookup"><span data-stu-id="a5dc2-165">Property</span></span>                | <span data-ttu-id="a5dc2-166">Descrição</span><span class="sxs-lookup"><span data-stu-id="a5dc2-166">Description</span></span>                       | <span data-ttu-id="a5dc2-167">Padrão</span><span class="sxs-lookup"><span data-stu-id="a5dc2-167">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `Cookie.Name`  | <span data-ttu-id="a5dc2-168">O nome do cookie.</span><span class="sxs-lookup"><span data-stu-id="a5dc2-168">The name of the cookie.</span></span>  | <span data-ttu-id="a5dc2-169">. AspNetCore.Cookies.</span><span class="sxs-lookup"><span data-stu-id="a5dc2-169">.AspNetCore.Cookies.</span></span>  |
| `Cookie.HttpOnly`  | <span data-ttu-id="a5dc2-170">Quando for verdadeiro, o cookie não é acessível a partir de scripts do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="a5dc2-170">When true, the cookie is not accessible from client-side scripts.</span></span>  |  <span data-ttu-id="a5dc2-171">true</span><span class="sxs-lookup"><span data-stu-id="a5dc2-171">true</span></span> |
| `ExpireTimeSpan`  | <span data-ttu-id="a5dc2-172">Controla o tempo que o tíquete de autenticação é armazenado no cookie permanecerá válido do ponto em que ele é criado.</span><span class="sxs-lookup"><span data-stu-id="a5dc2-172">Controls how much time the authentication ticket stored in the cookie will remain valid from the point it is created.</span></span>  | <span data-ttu-id="a5dc2-173">14 dias</span><span class="sxs-lookup"><span data-stu-id="a5dc2-173">14 days</span></span>  |
| `LoginPath`  | <span data-ttu-id="a5dc2-174">Quando um usuário está autorizado, ele será redirecionado para esse caminho para fazer logon.</span><span class="sxs-lookup"><span data-stu-id="a5dc2-174">When a user is unauthorized, they will be redirected to this path to login.</span></span> | <span data-ttu-id="a5dc2-175">/ / Logon da conta</span><span class="sxs-lookup"><span data-stu-id="a5dc2-175">/Account/Login</span></span>  |
| `LogoutPath`  | <span data-ttu-id="a5dc2-176">Quando um usuário é desconectado, ele será redirecionado para esse caminho.</span><span class="sxs-lookup"><span data-stu-id="a5dc2-176">When a user is logged out, they will be redirected to this path.</span></span>  | <span data-ttu-id="a5dc2-177">/ Conta/Logout</span><span class="sxs-lookup"><span data-stu-id="a5dc2-177">/Account/Logout</span></span>  |
| `AccessDeniedPath`  | <span data-ttu-id="a5dc2-178">Quando um usuário executar uma verificação de autorização, ele será redirecionado para esse caminho.</span><span class="sxs-lookup"><span data-stu-id="a5dc2-178">When a user fails an authorization check, they will be redirected to this path.</span></span>  |   |
| `SlidingExpiration`  | <span data-ttu-id="a5dc2-179">Quando for verdadeiro, será emitido um novo cookie com um novo tempo de expiração quando o cookie atual é mais de meio a janela de expiração.</span><span class="sxs-lookup"><span data-stu-id="a5dc2-179">When true, a new cookie will be issued with a new expiration time when the current cookie is more than halfway through the expiration window.</span></span>  | <span data-ttu-id="a5dc2-180">/ Conta/AccessDenied</span><span class="sxs-lookup"><span data-stu-id="a5dc2-180">/Account/AccessDenied</span></span> |
| `ReturnUrlParameter`  | <span data-ttu-id="a5dc2-181">Determina o nome do parâmetro de cadeia de caracteres de consulta que é acrescentado pelo middleware quando um código de 401 status não autorizado é alterado para um redirecionamento 302 para o caminho de logon.</span><span class="sxs-lookup"><span data-stu-id="a5dc2-181">Determines the name of the query string parameter which is appended by the middleware when a 401 Unauthorized status code is changed to a 302 redirect onto the login path.</span></span>  |  <span data-ttu-id="a5dc2-182">true</span><span class="sxs-lookup"><span data-stu-id="a5dc2-182">true</span></span> |
| `AuthenticationScheme`  | <span data-ttu-id="a5dc2-183">Isso só é relevante para o ASP.NET Core 1. x.</span><span class="sxs-lookup"><span data-stu-id="a5dc2-183">This is only relevant for ASP.NET Core 1.x.</span></span> <span data-ttu-id="a5dc2-184">O nome lógico para um esquema de autenticação específico.</span><span class="sxs-lookup"><span data-stu-id="a5dc2-184">The logical name for a particular authentication scheme.</span></span> |  |
| `AutomaticAuthenticate`  | <span data-ttu-id="a5dc2-185">Esse sinalizador só é relevante para o ASP.NET Core 1. x.</span><span class="sxs-lookup"><span data-stu-id="a5dc2-185">This flag is only relevant for ASP.NET Core 1.x.</span></span> <span data-ttu-id="a5dc2-186">Quando for verdadeiro, autenticação de cookie deve executar em cada solicitação e tente validar e reconstrua qualquer entidade de segurança serializada criado por ele.</span><span class="sxs-lookup"><span data-stu-id="a5dc2-186">When true, cookie authentication should run on every request and attempt to validate and reconstruct any serialized principal it created.</span></span>  |  |