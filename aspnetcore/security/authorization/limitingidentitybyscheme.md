---
title: "Limitação de identidade pelo esquema"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: d3d6ca1b-b4b5-4bf7-898e-dcd90ec1bf8c
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/limitingidentitybyscheme
ms.openlocfilehash: 2483c441da317a5c29b611b3a4910eae3c01fd7a
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2017
---
# <a name="limiting-identity-by-scheme"></a><span data-ttu-id="cce68-103">Limitação de identidade pelo esquema</span><span class="sxs-lookup"><span data-stu-id="cce68-103">Limiting identity by scheme</span></span>

<a name=security-authorization-limiting-by-scheme></a>

<span data-ttu-id="cce68-104">Em alguns cenários, como aplicativos de página única é possível acabar com vários métodos de autenticação.</span><span class="sxs-lookup"><span data-stu-id="cce68-104">In some scenarios, such as Single Page Applications it is possible to end up with multiple authentication methods.</span></span> <span data-ttu-id="cce68-105">Por exemplo, seu aplicativo pode usar autenticação baseada em cookie para fazer logon e autenticação do portador para solicitações de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="cce68-105">For example, your application may use cookie-based authentication to log in and bearer authentication for JavaScript requests.</span></span> <span data-ttu-id="cce68-106">Em alguns casos, você pode ter várias instâncias de um middleware de autenticação.</span><span class="sxs-lookup"><span data-stu-id="cce68-106">In some cases you may have multiple instances of an authentication middleware.</span></span> <span data-ttu-id="cce68-107">Por exemplo, dois cookies middlewares onde um contém uma identidade básica e um é criado quando uma autenticação multifator foi disparado porque o usuário solicitou uma operação que requer segurança adicional.</span><span class="sxs-lookup"><span data-stu-id="cce68-107">For example, two cookie middlewares where one contains a basic identity and one is created when a multi-factor authentication has triggered because the user requested an operation that requires extra security.</span></span>

<span data-ttu-id="cce68-108">Esquemas de autenticação são nomeados ao middleware de autenticação é configurado durante a autenticação, por exemplo</span><span class="sxs-lookup"><span data-stu-id="cce68-108">Authentication schemes are named when authentication middleware is configured during authentication, for example</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions()
{
    AuthenticationScheme = "Cookie",
    LoginPath = new PathString("/Account/Unauthorized/"),
    AccessDeniedPath = new PathString("/Account/Forbidden/"),
    AutomaticAuthenticate = false
});

app.UseBearerAuthentication(options =>
{
    options.AuthenticationScheme = "Bearer";
    options.AutomaticAuthenticate = false;
});
```

<span data-ttu-id="cce68-109">Nessa configuração middlewares de autenticação de dois foram adicionados, uma para cookies e outra para transmissão.</span><span class="sxs-lookup"><span data-stu-id="cce68-109">In this configuration two authentication middlewares have been added, one for cookies and one for bearer.</span></span>

>[!NOTE]
><span data-ttu-id="cce68-110">Ao adicionar vários middleware de autenticação, que você deve garantir que nenhum middleware é configurado para ser executado automaticamente.</span><span class="sxs-lookup"><span data-stu-id="cce68-110">When adding multiple authentication middleware you should ensure that no middleware is configured to run automatically.</span></span> <span data-ttu-id="cce68-111">Você pode fazer isso definindo o `AutomaticAuthenticate` opções de propriedade como false.</span><span class="sxs-lookup"><span data-stu-id="cce68-111">You do this by setting the `AutomaticAuthenticate` options property to false.</span></span> <span data-ttu-id="cce68-112">Se você não conseguir fazer a filtragem por esquema não funcionará.</span><span class="sxs-lookup"><span data-stu-id="cce68-112">If you fail to do this filtering by scheme will not work.</span></span>

## <a name="selecting-the-scheme-with-the-authorize-attribute"></a><span data-ttu-id="cce68-113">Selecionar o esquema com o atributo de autorização</span><span class="sxs-lookup"><span data-stu-id="cce68-113">Selecting the scheme with the Authorize attribute</span></span>

<span data-ttu-id="cce68-114">Como nenhuma middleware de autenticação está configurado para executar automaticamente e criar uma identidade, que você deve, no ponto de autorização escolha qual middleware será usado.</span><span class="sxs-lookup"><span data-stu-id="cce68-114">As no authentication middleware is configured to automatically run and create an identity you must, at the point of authorization choose which middleware will be used.</span></span> <span data-ttu-id="cce68-115">Selecione o middleware que deseja autorizar com a maneira mais simples é usar o `ActiveAuthenticationSchemes` propriedade.</span><span class="sxs-lookup"><span data-stu-id="cce68-115">The simplest way to select the middleware you wish to authorize with is to use the `ActiveAuthenticationSchemes` property.</span></span> <span data-ttu-id="cce68-116">Essa propriedade aceita uma lista delimitada por vírgulas de esquemas de autenticação a ser usado.</span><span class="sxs-lookup"><span data-stu-id="cce68-116">This property accepts a comma delimited list of Authentication Schemes to use.</span></span> <span data-ttu-id="cce68-117">Por exemplo,</span><span class="sxs-lookup"><span data-stu-id="cce68-117">For example;</span></span>

```csharp
[Authorize(ActiveAuthenticationSchemes = "Cookie,Bearer")]
public class MixedController : Controller
```

<span data-ttu-id="cce68-118">No exemplo acima de cookie e o portador middlewares será executado e tenha a oportunidade de criar e acrescentar uma identidade para o usuário atual.</span><span class="sxs-lookup"><span data-stu-id="cce68-118">In the example above both the cookie and bearer middlewares will run and have a chance to create and append an identity for the current user.</span></span> <span data-ttu-id="cce68-119">Especificando um único esquema somente o middleware especificado será executado;</span><span class="sxs-lookup"><span data-stu-id="cce68-119">By specifying a single scheme only the specified middleware will run;</span></span>

```csharp
[Authorize(ActiveAuthenticationSchemes = "Bearer")]
```

<span data-ttu-id="cce68-120">Nesse caso somente o middleware com o esquema de portador executaria e identidades baseada em cookie serão ignoradas.</span><span class="sxs-lookup"><span data-stu-id="cce68-120">In this case only the middleware with the Bearer scheme would run, and any cookie based identities would be ignored.</span></span>

## <a name="selecting-the-scheme-with-policies"></a><span data-ttu-id="cce68-121">Selecionar o esquema com as políticas</span><span class="sxs-lookup"><span data-stu-id="cce68-121">Selecting the scheme with policies</span></span>

<span data-ttu-id="cce68-122">Se você preferir especificar os esquemas de desejado [política](policies.md#security-authorization-policies-based) você pode definir o `AuthenticationSchemes` coleção ao adicionar sua política.</span><span class="sxs-lookup"><span data-stu-id="cce68-122">If you prefer to specify the desired schemes in [policy](policies.md#security-authorization-policies-based) you can set the `AuthenticationSchemes` collection when adding your policy.</span></span>

```csharp
options.AddPolicy("Over18", policy =>
{
    policy.AuthenticationSchemes.Add("Bearer");
    policy.RequireAuthenticatedUser();
    policy.Requirements.Add(new Over18Requirement());
});
```

<span data-ttu-id="cce68-123">Neste exemplo o Over18 política só será executada em relação a identidade criada pelo `Bearer` middleware.</span><span class="sxs-lookup"><span data-stu-id="cce68-123">In this example the Over18 policy will only run against the identity created by the `Bearer` middleware.</span></span>
