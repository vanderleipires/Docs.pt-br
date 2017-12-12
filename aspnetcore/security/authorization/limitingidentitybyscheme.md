---
title: "Autorizar com um esquema específico - ASP.NET Core"
author: rick-anderson
description: "Este artigo explica como limitar a identidade de um esquema específico ao trabalhar com vários métodos de autenticação."
keywords: "ASP.NET Core, identidade, o esquema de autenticação"
ms.author: riande
manager: wpickett
ms.date: 10/12/2017
ms.topic: article
ms.assetid: d3d6ca1b-b4b5-4bf7-898e-dcd90ec1bf8c
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/limitingidentitybyscheme
ms.openlocfilehash: 8c9d068b88263d0c06b11a6b87416fb02885c475
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
# <a name="authorize-with-a-specific-scheme"></a><span data-ttu-id="8869e-104">Autorizar com um esquema específico</span><span class="sxs-lookup"><span data-stu-id="8869e-104">Authorize with a specific scheme</span></span>

<span data-ttu-id="8869e-105">Em alguns cenários, como aplicativos de página única (SPAs), é comum usar vários métodos de autenticação.</span><span class="sxs-lookup"><span data-stu-id="8869e-105">In some scenarios, such as Single Page Applications (SPAs), it's common to use multiple authentication methods.</span></span> <span data-ttu-id="8869e-106">Por exemplo, o aplicativo pode usar autenticação baseada em cookie para fazer logon e autenticação do portador JWT para solicitações de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="8869e-106">For example, the app may use cookie-based authentication to log in and JWT bearer authentication for JavaScript requests.</span></span> <span data-ttu-id="8869e-107">Em alguns casos, o aplicativo pode ter várias instâncias de um manipulador de autenticação.</span><span class="sxs-lookup"><span data-stu-id="8869e-107">In some cases, the app may have multiple instances of an authentication handler.</span></span> <span data-ttu-id="8869e-108">Por exemplo, dois manipuladores de cookie onde um contém uma identidade básica e um é criado quando uma autenticação multifator (MFA) foi acionada.</span><span class="sxs-lookup"><span data-stu-id="8869e-108">For example, two cookie handlers where one contains a basic identity and one is created when a multi-factor authentication (MFA) has been triggered.</span></span> <span data-ttu-id="8869e-109">MFA pode ser acionado porque o usuário solicitou uma operação que requer segurança adicional.</span><span class="sxs-lookup"><span data-stu-id="8869e-109">MFA may be triggered because the user requested an operation that requires extra security.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8869e-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8869e-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="8869e-111">Um esquema de autenticação é chamado quando o serviço de autenticação é configurado durante a autenticação.</span><span class="sxs-lookup"><span data-stu-id="8869e-111">An authentication scheme is named when the authentication service is configured during authentication.</span></span> <span data-ttu-id="8869e-112">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="8869e-112">For example:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Code omitted for brevity

    services.AddAuthentication()
        .AddCookie(options => {
            options.LoginPath = "/Account/Unauthorized/";
            options.AccessDeniedPath = "/Account/Forbidden/";
        })
        .AddJwtBearer(options => {
            options.Audience = "http://localhost:5001/";
            options.Authority = "http://localhost:5000/";
        });
```

<span data-ttu-id="8869e-113">No código anterior, foram adicionados dois manipuladores de autenticação: um para cookies e outro para transmissão.</span><span class="sxs-lookup"><span data-stu-id="8869e-113">In the preceding code, two authentication handlers have been added: one for cookies and one for bearer.</span></span>

>[!NOTE]
><span data-ttu-id="8869e-114">Especifica o esquema padrão resulta no `HttpContext.User` propriedade sendo definida como essa identidade.</span><span class="sxs-lookup"><span data-stu-id="8869e-114">Specifying the default scheme results in the `HttpContext.User` property being set to that identity.</span></span> <span data-ttu-id="8869e-115">Se esse comportamento não for desejado, desabilite-o invocando a forma sem parâmetros de `AddAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="8869e-115">If that behavior isn't desired, disable it by invoking the parameterless form of `AddAuthentication`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8869e-116">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8869e-116">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="8869e-117">Esquemas de autenticação são nomeados quando middlewares de autenticação são configurados durante a autenticação.</span><span class="sxs-lookup"><span data-stu-id="8869e-117">Authentication schemes are named when authentication middlewares are configured during authentication.</span></span> <span data-ttu-id="8869e-118">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="8869e-118">For example:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
{
    // Code omitted for brevity

    app.UseCookieAuthentication(new CookieAuthenticationOptions()
    {
        AuthenticationScheme = "Cookie",
        LoginPath = "/Account/Unauthorized/",
        AccessDeniedPath = "/Account/Forbidden/",
        AutomaticAuthenticate = false
    });
    
    app.UseJwtBearerAuthentication(new JwtBearerOptions()
    {
        AuthenticationScheme = "Bearer",
        AutomaticAuthenticate = false,
        Audience = "http://localhost:5001/",
        Authority = "http://localhost:5000/",
        RequireHttpsMetadata = false
    });
```

<span data-ttu-id="8869e-119">No código anterior, foram adicionados dois middlewares de autenticação: um para cookies e outro para transmissão.</span><span class="sxs-lookup"><span data-stu-id="8869e-119">In the preceding code, two authentication middlewares have been added: one for cookies and one for bearer.</span></span>

>[!NOTE]
><span data-ttu-id="8869e-120">Especifica o esquema padrão resulta no `HttpContext.User` propriedade sendo definida como essa identidade.</span><span class="sxs-lookup"><span data-stu-id="8869e-120">Specifying the default scheme results in the `HttpContext.User` property being set to that identity.</span></span> <span data-ttu-id="8869e-121">Se esse comportamento não for desejado, desabilite-o definindo o `AuthenticationOptions.AutomaticAuthenticate` propriedade `false`.</span><span class="sxs-lookup"><span data-stu-id="8869e-121">If that behavior isn't desired, disable it by setting the `AuthenticationOptions.AutomaticAuthenticate` property to `false`.</span></span>

---

## <a name="selecting-the-scheme-with-the-authorize-attribute"></a><span data-ttu-id="8869e-122">Selecionar o esquema com o atributo de autorização</span><span class="sxs-lookup"><span data-stu-id="8869e-122">Selecting the scheme with the Authorize attribute</span></span>

<span data-ttu-id="8869e-123">No ponto de autorização, o aplicativo indica o manipulador a ser usado.</span><span class="sxs-lookup"><span data-stu-id="8869e-123">At the point of authorization, the app indicates the handler to be used.</span></span> <span data-ttu-id="8869e-124">Selecione o manipulador com o qual o aplicativo autorizará passando uma lista delimitada por vírgulas de esquemas de autenticação para `[Authorize]`.</span><span class="sxs-lookup"><span data-stu-id="8869e-124">Select the handler with which the app will authorize by passing a comma-delimited list of authentication schemes to `[Authorize]`.</span></span> <span data-ttu-id="8869e-125">O `[Authorize]` atributo especifica o esquema de autenticação ou esquemas de usar, independentemente se um padrão está configurado.</span><span class="sxs-lookup"><span data-stu-id="8869e-125">The `[Authorize]` attribute specifies the authentication scheme or schemes to use regardless of whether a default is configured.</span></span> <span data-ttu-id="8869e-126">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="8869e-126">For example:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8869e-127">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8869e-127">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
[Authorize(AuthenticationSchemes = AuthSchemes)]
public class MixedController : Controller
    // Requires the following imports:
    // using Microsoft.AspNetCore.Authentication.Cookies;
    // using Microsoft.AspNetCore.Authentication.JwtBearer;
    private const string AuthSchemes =
        CookieAuthenticationDefaults.AuthenticationScheme + "," +
        JwtBearerDefaults.AuthenticationScheme;
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8869e-128">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8869e-128">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
[Authorize(ActiveAuthenticationSchemes = AuthSchemes)]
public class MixedController : Controller
    // Requires the following imports:
    // using Microsoft.AspNetCore.Authentication.Cookies;
    // using Microsoft.AspNetCore.Authentication.JwtBearer;
    private const string AuthSchemes =
        CookieAuthenticationDefaults.AuthenticationScheme + "," +
        JwtBearerDefaults.AuthenticationScheme;
```

---

<span data-ttu-id="8869e-129">No exemplo anterior, o cookie e o portador manipuladores execute em terá a oportunidade de criar e acrescentar uma identidade para o usuário atual.</span><span class="sxs-lookup"><span data-stu-id="8869e-129">In the preceding example, both the cookie and bearer handlers run and have a chance to create and append an identity for the current user.</span></span> <span data-ttu-id="8869e-130">Ao especificar um único esquema, o manipulador correspondente é executado.</span><span class="sxs-lookup"><span data-stu-id="8869e-130">By specifying a single scheme only, the corresponding handler runs.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8869e-131">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8869e-131">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
[Authorize(AuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8869e-132">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8869e-132">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
[Authorize(ActiveAuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

---

<span data-ttu-id="8869e-133">No código acima, somente o manipulador com o esquema de "Portador" em execução.</span><span class="sxs-lookup"><span data-stu-id="8869e-133">In the preceding code, only the handler with the "Bearer" scheme runs.</span></span> <span data-ttu-id="8869e-134">Todas as identidades baseada em cookie são ignoradas.</span><span class="sxs-lookup"><span data-stu-id="8869e-134">Any cookie-based identities are ignored.</span></span>

## <a name="selecting-the-scheme-with-policies"></a><span data-ttu-id="8869e-135">Selecionar o esquema com as políticas</span><span class="sxs-lookup"><span data-stu-id="8869e-135">Selecting the scheme with policies</span></span>

<span data-ttu-id="8869e-136">Se você preferir especificar os esquemas de desejado [política](xref:security/authorization/policies), você pode definir o `AuthenticationSchemes` ao adicionar sua política de coleta:</span><span class="sxs-lookup"><span data-stu-id="8869e-136">If you prefer to specify the desired schemes in [policy](xref:security/authorization/policies), you can set the `AuthenticationSchemes` collection when adding your policy:</span></span>

```csharp
services.AddAuthorization(options =>
{
    options.AddPolicy("Over18", policy =>
    {
        policy.AuthenticationSchemes.Add(JwtBearerDefaults.AuthenticationScheme);
        policy.RequireAuthenticatedUser();
        policy.Requirements.Add(new MinimumAgeRequirement());
    });
});
```

<span data-ttu-id="8869e-137">No exemplo anterior, a política de "O Over18" só é executada em relação a identidade criada pelo manipulador de "Portador".</span><span class="sxs-lookup"><span data-stu-id="8869e-137">In the preceding example, the "Over18" policy only runs against the identity created by the "Bearer" handler.</span></span> <span data-ttu-id="8869e-138">Usar a política, definindo o `[Authorize]` do atributo `Policy` propriedade:</span><span class="sxs-lookup"><span data-stu-id="8869e-138">Use the policy by setting the `[Authorize]` attribute's `Policy` property:</span></span>

```csharp
[Authorize(Policy = "Over18")]
public class RegistrationController : Controller
```
