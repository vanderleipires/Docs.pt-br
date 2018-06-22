---
title: Convenções de autorização de páginas Razor do ASP.NET Core
author: guardrex
description: Aprenda a controlar o acesso a páginas com as convenções de autorizam usuários e permitir que usuários anônimos acessem pastas de páginas ou de páginas.
ms.author: riande
ms.custom: mvc
ms.date: 10/27/2017
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: 8856520bf43f2f62cc12c7e883485babdb43fb3e
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272669"
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a><span data-ttu-id="f169e-103">Convenções de autorização de páginas Razor do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f169e-103">Razor Pages authorization conventions in ASP.NET Core</span></span>

<span data-ttu-id="f169e-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="f169e-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="f169e-105">Uma maneira de controlar o acesso em seu aplicativo de páginas Razor é usar as convenções de autorização na inicialização.</span><span class="sxs-lookup"><span data-stu-id="f169e-105">One way to control access in your Razor Pages app is to use authorization conventions at startup.</span></span> <span data-ttu-id="f169e-106">As seguintes convenções permitem que você autorizar os usuários e permitir que usuários anônimos acessem páginas individuais ou pastas de páginas.</span><span class="sxs-lookup"><span data-stu-id="f169e-106">These conventions allow you to authorize users and allow anonymous users to access individual pages or folders of pages.</span></span> <span data-ttu-id="f169e-107">Aplicam as convenções descritas neste tópico automaticamente [filtros de autorização](xref:mvc/controllers/filters#authorization-filters) para controlar o acesso.</span><span class="sxs-lookup"><span data-stu-id="f169e-107">The conventions described in this topic automatically apply [authorization filters](xref:mvc/controllers/filters#authorization-filters) to control access.</span></span>

<span data-ttu-id="f169e-108">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f169e-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="f169e-109">O aplicativo de exemplo usa [autenticação de Cookie sem a identidade do ASP.NET Core](xref:security/authentication/cookie).</span><span class="sxs-lookup"><span data-stu-id="f169e-109">The sample app uses [Cookie authentication without ASP.NET Core Identity](xref:security/authentication/cookie).</span></span> <span data-ttu-id="f169e-110">A conta de usuário para o usuário hipotético, Maria Rodriguez, está codificada no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="f169e-110">The user account for the hypothetical user, Maria Rodriguez, is hardcoded into the app.</span></span> <span data-ttu-id="f169e-111">Use o nome de usuário de Email "maria.rodriguez@contoso.com" e uma senha para entrar no usuário.</span><span class="sxs-lookup"><span data-stu-id="f169e-111">Use the Email username "maria.rodriguez@contoso.com" and any password to sign in the user.</span></span> <span data-ttu-id="f169e-112">O usuário é autenticado no `AuthenticateUser` método o *Pages/Account/Login.cshtml.cs* arquivo.</span><span class="sxs-lookup"><span data-stu-id="f169e-112">The user is authenticated in the `AuthenticateUser` method in the *Pages/Account/Login.cshtml.cs* file.</span></span> <span data-ttu-id="f169e-113">Em um exemplo do mundo real, o usuário deve ser autenticado em um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f169e-113">In a real-world example, the user would be authenticated against a database.</span></span> <span data-ttu-id="f169e-114">Para usar a identidade do ASP.NET Core, siga as orientações de [Introdução a identidade do ASP.NET Core](xref:security/authentication/identity) tópico.</span><span class="sxs-lookup"><span data-stu-id="f169e-114">To use ASP.NET Core Identity, follow the guidance in the [Introduction to Identity on ASP.NET Core](xref:security/authentication/identity) topic.</span></span> <span data-ttu-id="f169e-115">Os conceitos e os exemplos mostrados neste tópico se aplicam igualmente a aplicativos que usam a identidade do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f169e-115">The concepts and examples shown in this topic apply equally to apps that use ASP.NET Core Identity.</span></span>

## <a name="require-authorization-to-access-a-page"></a><span data-ttu-id="f169e-116">Exigir autorização para acessar uma página</span><span class="sxs-lookup"><span data-stu-id="f169e-116">Require authorization to access a page</span></span>

<span data-ttu-id="f169e-117">Use o [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) convenção via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) para adicionar um [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) para a página no caminho especificado:</span><span class="sxs-lookup"><span data-stu-id="f169e-117">Use the [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to the page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

<span data-ttu-id="f169e-118">O caminho especificado é o caminho do mecanismo de exibição, que é o caminho relativo do Razor páginas raiz sem uma extensão e contendo apenas barras invertidas.</span><span class="sxs-lookup"><span data-stu-id="f169e-118">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

<span data-ttu-id="f169e-119">Um [AuthorizePage sobrecarga](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) estará disponível se você precisa especificar uma política de autorização.</span><span class="sxs-lookup"><span data-stu-id="f169e-119">An [AuthorizePage overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

::: moniker range=">= aspnetcore-2.1"

> [!NOTE]
> <span data-ttu-id="f169e-120">Um `AuthorizeFilter` pode ser aplicado a uma classe de modelo de página com o `[Authorize]` atributo de filtro.</span><span class="sxs-lookup"><span data-stu-id="f169e-120">An `AuthorizeFilter` can be applied to a page model class with the `[Authorize]` filter attribute.</span></span> <span data-ttu-id="f169e-121">Para obter mais informações, consulte [atributo de filtro de autorização](xref:razor-pages/filter#authorize-filter-attribute).</span><span class="sxs-lookup"><span data-stu-id="f169e-121">For more information, see [Authorize filter attribute](xref:razor-pages/filter#authorize-filter-attribute).</span></span>

::: moniker-end

## <a name="require-authorization-to-access-a-folder-of-pages"></a><span data-ttu-id="f169e-122">Exigir autorização para acessar uma pasta de páginas</span><span class="sxs-lookup"><span data-stu-id="f169e-122">Require authorization to access a folder of pages</span></span>

<span data-ttu-id="f169e-123">Use o [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) convenção via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) para adicionar um [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) para todas as páginas em uma pasta no caminho especificado:</span><span class="sxs-lookup"><span data-stu-id="f169e-123">Use the [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

<span data-ttu-id="f169e-124">O caminho especificado é o caminho do mecanismo de exibição, que é o caminho relativo de raiz de páginas Razor.</span><span class="sxs-lookup"><span data-stu-id="f169e-124">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

<span data-ttu-id="f169e-125">Um [AuthorizeFolder sobrecarga](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) estará disponível se você precisa especificar uma política de autorização.</span><span class="sxs-lookup"><span data-stu-id="f169e-125">An [AuthorizeFolder overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

## <a name="allow-anonymous-access-to-a-page"></a><span data-ttu-id="f169e-126">Permitir acesso anônimo a uma página</span><span class="sxs-lookup"><span data-stu-id="f169e-126">Allow anonymous access to a page</span></span>

<span data-ttu-id="f169e-127">Use o [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) convenção via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) para adicionar um [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) para uma página no caminho especificado:</span><span class="sxs-lookup"><span data-stu-id="f169e-127">Use the [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) to a page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

<span data-ttu-id="f169e-128">O caminho especificado é o caminho do mecanismo de exibição, que é o caminho relativo do Razor páginas raiz sem uma extensão e contendo apenas barras invertidas.</span><span class="sxs-lookup"><span data-stu-id="f169e-128">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a><span data-ttu-id="f169e-129">Permitir o acesso anônimo para uma pasta de páginas</span><span class="sxs-lookup"><span data-stu-id="f169e-129">Allow anonymous access to a folder of pages</span></span>

<span data-ttu-id="f169e-130">Use o [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) convenção via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) para adicionar um [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) para todas as páginas em uma pasta no caminho especificado:</span><span class="sxs-lookup"><span data-stu-id="f169e-130">Use the [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

<span data-ttu-id="f169e-131">O caminho especificado é o caminho do mecanismo de exibição, que é o caminho relativo de raiz de páginas Razor.</span><span class="sxs-lookup"><span data-stu-id="f169e-131">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

## <a name="note-on-combining-authorized-and-anonymous-access"></a><span data-ttu-id="f169e-132">Observe na combinação de autorização e acesso anônimo</span><span class="sxs-lookup"><span data-stu-id="f169e-132">Note on combining authorized and anonymous access</span></span>

<span data-ttu-id="f169e-133">É perfeitamente válido para especificar que uma pasta de páginas exigem autorização e especifique que uma página dentro da pasta permite acesso anônimo:</span><span class="sxs-lookup"><span data-stu-id="f169e-133">It's perfectly valid to specify that a folder of pages require authorization and specify that a page within that folder allows anonymous access:</span></span>

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

<span data-ttu-id="f169e-134">O inverso, no entanto, não é true.</span><span class="sxs-lookup"><span data-stu-id="f169e-134">The reverse, however, isn't true.</span></span> <span data-ttu-id="f169e-135">Você não pode declarar uma pasta de páginas para acesso anônimo e especificar uma página dentro de autorização:</span><span class="sxs-lookup"><span data-stu-id="f169e-135">You can't declare a folder of pages for anonymous access and specify a page within for authorization:</span></span>

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private") 
```

<span data-ttu-id="f169e-136">Exigir autorização na página privada não funcionará porque quando tanto o `AllowAnonymousFilter` e `AuthorizeFilter` filtros são aplicados para a página, o `AllowAnonymousFilter` wins e controla o acesso.</span><span class="sxs-lookup"><span data-stu-id="f169e-136">Requiring authorization on the Private page won't work because when both the `AllowAnonymousFilter` and `AuthorizeFilter` filters are applied to the page, the `AllowAnonymousFilter` wins and controls access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f169e-137">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="f169e-137">Additional resources</span></span>

* [<span data-ttu-id="f169e-138">Provedores de modelo personalizado de página e rota de Páginas Razor</span><span class="sxs-lookup"><span data-stu-id="f169e-138">Razor Pages custom route and page model providers</span></span>](xref:razor-pages/razor-pages-conventions)
* <span data-ttu-id="f169e-139">[PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) classe</span><span class="sxs-lookup"><span data-stu-id="f169e-139">[PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) class</span></span>
