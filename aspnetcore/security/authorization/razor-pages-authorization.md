---
title: Convenções de autorização de páginas do Razor no ASP.NET Core
author: guardrex
description: Saiba como controlar o acesso a páginas com as convenções de autorizam usuários e permitir que usuários anônimos acessem páginas ou pastas de páginas.
ms.author: riande
ms.custom: mvc
ms.date: 10/27/2017
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: d3ecb41765da912df68aeb829350d27e4d087e3a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824671"
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a><span data-ttu-id="9e77d-103">Convenções de autorização de páginas do Razor no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9e77d-103">Razor Pages authorization conventions in ASP.NET Core</span></span>

<span data-ttu-id="9e77d-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="9e77d-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="9e77d-105">Uma maneira de controlar o acesso em seu aplicativo de páginas do Razor é usar as convenções de autorização na inicialização.</span><span class="sxs-lookup"><span data-stu-id="9e77d-105">One way to control access in your Razor Pages app is to use authorization conventions at startup.</span></span> <span data-ttu-id="9e77d-106">Essas convenções permitem que você autorizar usuários e permitir que usuários anônimos acessem páginas individuais ou pastas de páginas.</span><span class="sxs-lookup"><span data-stu-id="9e77d-106">These conventions allow you to authorize users and allow anonymous users to access individual pages or folders of pages.</span></span> <span data-ttu-id="9e77d-107">Aplicam as convenções descritas neste tópico automaticamente [filtros de autorização](xref:mvc/controllers/filters#authorization-filters) para controlar o acesso.</span><span class="sxs-lookup"><span data-stu-id="9e77d-107">The conventions described in this topic automatically apply [authorization filters](xref:mvc/controllers/filters#authorization-filters) to control access.</span></span>

<span data-ttu-id="9e77d-108">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9e77d-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="9e77d-109">O aplicativo de exemplo usa [autenticação de Cookie sem o ASP.NET Core Identity](xref:security/authentication/cookie).</span><span class="sxs-lookup"><span data-stu-id="9e77d-109">The sample app uses [Cookie authentication without ASP.NET Core Identity](xref:security/authentication/cookie).</span></span> <span data-ttu-id="9e77d-110">A conta de usuário para o usuário hipotético, Maria Rodriguez, é codificados no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="9e77d-110">The user account for the hypothetical user, Maria Rodriguez, is hardcoded into the app.</span></span> <span data-ttu-id="9e77d-111">Use o nome de usuário de Email "maria.rodriguez@contoso.com" e nenhuma senha para a entrada do usuário.</span><span class="sxs-lookup"><span data-stu-id="9e77d-111">Use the Email username "maria.rodriguez@contoso.com" and any password to sign in the user.</span></span> <span data-ttu-id="9e77d-112">O usuário é autenticado na `AuthenticateUser` método na *Pages/Account/Login.cshtml.cs* arquivo.</span><span class="sxs-lookup"><span data-stu-id="9e77d-112">The user is authenticated in the `AuthenticateUser` method in the *Pages/Account/Login.cshtml.cs* file.</span></span> <span data-ttu-id="9e77d-113">Em um exemplo do mundo real, o usuário deve ser autenticado em relação a um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="9e77d-113">In a real-world example, the user would be authenticated against a database.</span></span> <span data-ttu-id="9e77d-114">Para usar o ASP.NET Core Identity, siga as diretrizes a [Introdução à identidade no ASP.NET Core](xref:security/authentication/identity) tópico.</span><span class="sxs-lookup"><span data-stu-id="9e77d-114">To use ASP.NET Core Identity, follow the guidance in the [Introduction to Identity on ASP.NET Core](xref:security/authentication/identity) topic.</span></span> <span data-ttu-id="9e77d-115">Os conceitos e os exemplos mostrados neste tópico se aplicam igualmente a aplicativos que usam o ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="9e77d-115">The concepts and examples shown in this topic apply equally to apps that use ASP.NET Core Identity.</span></span>

## <a name="require-authorization-to-access-a-page"></a><span data-ttu-id="9e77d-116">Exigir autorização para acessar uma página</span><span class="sxs-lookup"><span data-stu-id="9e77d-116">Require authorization to access a page</span></span>

<span data-ttu-id="9e77d-117">Use o [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) convenção via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) para adicionar um [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) para a página no caminho especificado:</span><span class="sxs-lookup"><span data-stu-id="9e77d-117">Use the [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to the page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

<span data-ttu-id="9e77d-118">O caminho especificado é o caminho de mecanismo de exibição, que é o caminho relativo de raiz de páginas do Razor sem uma extensão e que contém apenas barras "/".</span><span class="sxs-lookup"><span data-stu-id="9e77d-118">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

<span data-ttu-id="9e77d-119">Uma [AuthorizePage sobrecarga](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) estará disponível se você precisa especificar uma política de autorização.</span><span class="sxs-lookup"><span data-stu-id="9e77d-119">An [AuthorizePage overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

::: moniker range=">= aspnetcore-2.1"

> [!NOTE]
> <span data-ttu-id="9e77d-120">Uma `AuthorizeFilter` pode ser aplicado a uma classe de modelo de página com o `[Authorize]` atributo de filtro.</span><span class="sxs-lookup"><span data-stu-id="9e77d-120">An `AuthorizeFilter` can be applied to a page model class with the `[Authorize]` filter attribute.</span></span> <span data-ttu-id="9e77d-121">Para obter mais informações, consulte [atributo de filtro Authorize](xref:razor-pages/filter#authorize-filter-attribute).</span><span class="sxs-lookup"><span data-stu-id="9e77d-121">For more information, see [Authorize filter attribute](xref:razor-pages/filter#authorize-filter-attribute).</span></span>

::: moniker-end

## <a name="require-authorization-to-access-a-folder-of-pages"></a><span data-ttu-id="9e77d-122">Exigir autorização para acessar uma pasta de páginas</span><span class="sxs-lookup"><span data-stu-id="9e77d-122">Require authorization to access a folder of pages</span></span>

<span data-ttu-id="9e77d-123">Use o [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) convenção via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) para adicionar um [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) para todas as páginas em uma pasta no caminho especificado:</span><span class="sxs-lookup"><span data-stu-id="9e77d-123">Use the [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

<span data-ttu-id="9e77d-124">O caminho especificado é o caminho de mecanismo de exibição, que é o caminho relativo de raiz de páginas do Razor.</span><span class="sxs-lookup"><span data-stu-id="9e77d-124">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

<span data-ttu-id="9e77d-125">Uma [AuthorizeFolder sobrecarga](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) estará disponível se você precisa especificar uma política de autorização.</span><span class="sxs-lookup"><span data-stu-id="9e77d-125">An [AuthorizeFolder overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="require-authorization-to-access-an-area-page"></a><span data-ttu-id="9e77d-126">Exigir autorização para acessar uma página de área</span><span class="sxs-lookup"><span data-stu-id="9e77d-126">Require authorization to access an area page</span></span>

<span data-ttu-id="9e77d-127">Use o [AuthorizeAreaPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareapage) convenção via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) para adicionar um [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) para a página de área no caminho especificado:</span><span class="sxs-lookup"><span data-stu-id="9e77d-127">Use the [AuthorizeAreaPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareapage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to the area page at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

<span data-ttu-id="9e77d-128">O nome da página é o caminho do arquivo sem uma extensão relativo ao diretório raiz páginas para a área especificada.</span><span class="sxs-lookup"><span data-stu-id="9e77d-128">The page name is the path of the file without an extension relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="9e77d-129">Por exemplo, o nome da página para o arquivo *Areas/Identity/Pages/Manage/Accounts.cshtml* é *contas/gerenciar/*.</span><span class="sxs-lookup"><span data-stu-id="9e77d-129">For example, the page name for the file *Areas/Identity/Pages/Manage/Accounts.cshtml* is */Manage/Accounts*.</span></span>

<span data-ttu-id="9e77d-130">Uma [AuthorizeAreaPage sobrecarga](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareapage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeAreaPage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_System_String_) estará disponível se você precisa especificar uma política de autorização.</span><span class="sxs-lookup"><span data-stu-id="9e77d-130">An [AuthorizeAreaPage overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareapage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeAreaPage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

## <a name="require-authorization-to-access-a-folder-of-areas"></a><span data-ttu-id="9e77d-131">Exigir autorização para acessar uma pasta de áreas</span><span class="sxs-lookup"><span data-stu-id="9e77d-131">Require authorization to access a folder of areas</span></span>

<span data-ttu-id="9e77d-132">Use o [AuthorizeAreaFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareafolder) convenção via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) para adicionar um [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) para todas as áreas em uma pasta no caminho especificado:</span><span class="sxs-lookup"><span data-stu-id="9e77d-132">Use the [AuthorizeAreaFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareafolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to all of the areas in a folder at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

<span data-ttu-id="9e77d-133">O caminho da pasta é o caminho da pasta, relativo ao diretório raiz páginas para a área especificada.</span><span class="sxs-lookup"><span data-stu-id="9e77d-133">The folder path is the path of the folder relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="9e77d-134">Por exemplo, o caminho da pasta para os arquivos sob *áreas/identidade/páginas/gerenciar/* é */gerenciar*.</span><span class="sxs-lookup"><span data-stu-id="9e77d-134">For example, the folder path for the files under *Areas/Identity/Pages/Manage/* is */Manage*.</span></span>

<span data-ttu-id="9e77d-135">Uma [AuthorizeAreaFolder sobrecarga](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareafolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeAreaFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_System_String_) estará disponível se você precisa especificar uma política de autorização.</span><span class="sxs-lookup"><span data-stu-id="9e77d-135">An [AuthorizeAreaFolder overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareafolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeAreaFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

::: moniker-end

## <a name="allow-anonymous-access-to-a-page"></a><span data-ttu-id="9e77d-136">Permitir o acesso anônimo para uma página</span><span class="sxs-lookup"><span data-stu-id="9e77d-136">Allow anonymous access to a page</span></span>

<span data-ttu-id="9e77d-137">Use o [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) convenção via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) para adicionar um [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) para uma página no caminho especificado:</span><span class="sxs-lookup"><span data-stu-id="9e77d-137">Use the [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) to a page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

<span data-ttu-id="9e77d-138">O caminho especificado é o caminho de mecanismo de exibição, que é o caminho relativo de raiz de páginas do Razor sem uma extensão e que contém apenas barras "/".</span><span class="sxs-lookup"><span data-stu-id="9e77d-138">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a><span data-ttu-id="9e77d-139">Permitir o acesso anônimo para uma pasta de páginas</span><span class="sxs-lookup"><span data-stu-id="9e77d-139">Allow anonymous access to a folder of pages</span></span>

<span data-ttu-id="9e77d-140">Use o [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) convenção via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) para adicionar um [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) para todas as páginas em uma pasta no caminho especificado:</span><span class="sxs-lookup"><span data-stu-id="9e77d-140">Use the [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

<span data-ttu-id="9e77d-141">O caminho especificado é o caminho de mecanismo de exibição, que é o caminho relativo de raiz de páginas do Razor.</span><span class="sxs-lookup"><span data-stu-id="9e77d-141">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

## <a name="note-on-combining-authorized-and-anonymous-access"></a><span data-ttu-id="9e77d-142">Observação sobre combinação autorizada e acesso anônimo</span><span class="sxs-lookup"><span data-stu-id="9e77d-142">Note on combining authorized and anonymous access</span></span>

<span data-ttu-id="9e77d-143">É perfeitamente válido para especificar que uma pasta de páginas exigem autorização e especificar uma página dentro da pasta permite o acesso anônimo:</span><span class="sxs-lookup"><span data-stu-id="9e77d-143">It's perfectly valid to specify that a folder of pages require authorization and specify that a page within that folder allows anonymous access:</span></span>

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

<span data-ttu-id="9e77d-144">O inverso, no entanto, não é verdadeiro.</span><span class="sxs-lookup"><span data-stu-id="9e77d-144">The reverse, however, isn't true.</span></span> <span data-ttu-id="9e77d-145">Você não pode declarar uma pasta de páginas para acesso anônimo e especificar uma página dentro de autorização:</span><span class="sxs-lookup"><span data-stu-id="9e77d-145">You can't declare a folder of pages for anonymous access and specify a page within for authorization:</span></span>

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private") 
```

<span data-ttu-id="9e77d-146">Exigir autorização na página privada não funcionará porque quando tanto o `AllowAnonymousFilter` e `AuthorizeFilter` filtros são aplicados para a página, o `AllowAnonymousFilter` wins e controla o acesso.</span><span class="sxs-lookup"><span data-stu-id="9e77d-146">Requiring authorization on the Private page won't work because when both the `AllowAnonymousFilter` and `AuthorizeFilter` filters are applied to the page, the `AllowAnonymousFilter` wins and controls access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9e77d-147">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="9e77d-147">Additional resources</span></span>

* [<span data-ttu-id="9e77d-148">Provedores de modelo personalizado de página e rota de Páginas Razor</span><span class="sxs-lookup"><span data-stu-id="9e77d-148">Razor Pages custom route and page model providers</span></span>](xref:razor-pages/razor-pages-conventions)
* <span data-ttu-id="9e77d-149">[PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) classe</span><span class="sxs-lookup"><span data-stu-id="9e77d-149">[PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) class</span></span>
