---
title: Convenções de autorização de páginas Razor do ASP.NET Core
author: guardrex
description: Aprenda a controlar o acesso a páginas com as convenções de autorizam usuários e permitir que usuários anônimos acessem pastas de páginas ou de páginas.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 10/27/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: cd1fa7957ca50db0de71f71234f84d3fbc631f45
ms.sourcegitcommit: 7e87671fea9a5f36ca516616fe3b40b537f428d2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/12/2018
ms.locfileid: "35341737"
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a>Convenções de autorização de páginas Razor do ASP.NET Core

Por [Luke Latham](https://github.com/guardrex)

Uma maneira de controlar o acesso em seu aplicativo de páginas Razor é usar as convenções de autorização na inicialização. As seguintes convenções permitem que você autorizar os usuários e permitir que usuários anônimos acessem páginas individuais ou pastas de páginas. Aplicam as convenções descritas neste tópico automaticamente [filtros de autorização](xref:mvc/controllers/filters#authorization-filters) para controlar o acesso.

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

O aplicativo de exemplo usa [autenticação de Cookie sem a identidade do ASP.NET Core](xref:security/authentication/cookie). A conta de usuário para o usuário hipotético, Maria Rodriguez, está codificada no aplicativo. Use o nome de usuário de Email "maria.rodriguez@contoso.com" e uma senha para entrar no usuário. O usuário é autenticado no `AuthenticateUser` método o *Pages/Account/Login.cshtml.cs* arquivo. Em um exemplo do mundo real, o usuário deve ser autenticado em um banco de dados. Para usar a identidade do ASP.NET Core, siga as orientações de [Introdução a identidade do ASP.NET Core](xref:security/authentication/identity) tópico. Os conceitos e os exemplos mostrados neste tópico se aplicam igualmente a aplicativos que usam a identidade do ASP.NET Core.

## <a name="require-authorization-to-access-a-page"></a>Exigir autorização para acessar uma página

Use o [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) convenção via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) para adicionar um [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) para a página no caminho especificado:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

O caminho especificado é o caminho do mecanismo de exibição, que é o caminho relativo do Razor páginas raiz sem uma extensão e contendo apenas barras invertidas.

Um [AuthorizePage sobrecarga](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) estará disponível se você precisa especificar uma política de autorização.

::: moniker range=">= aspnetcore-2.1"

> [!NOTE]
> Um `AuthorizeFilter` pode ser aplicado a uma classe de modelo de página com o `[Authorize]` atributo de filtro. Para obter mais informações, consulte [atributo de filtro de autorização](xref:mvc/razor-pages/filter#authorize-filter-attribute).

::: moniker-end

## <a name="require-authorization-to-access-a-folder-of-pages"></a>Exigir autorização para acessar uma pasta de páginas

Use o [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) convenção via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) para adicionar um [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) para todas as páginas em uma pasta no caminho especificado:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

O caminho especificado é o caminho do mecanismo de exibição, que é o caminho relativo de raiz de páginas Razor.

Um [AuthorizeFolder sobrecarga](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) estará disponível se você precisa especificar uma política de autorização.

## <a name="allow-anonymous-access-to-a-page"></a>Permitir acesso anônimo a uma página

Use o [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) convenção via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) para adicionar um [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) para uma página no caminho especificado:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

O caminho especificado é o caminho do mecanismo de exibição, que é o caminho relativo do Razor páginas raiz sem uma extensão e contendo apenas barras invertidas.

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a>Permitir o acesso anônimo para uma pasta de páginas

Use o [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) convenção via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) para adicionar um [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) para todas as páginas em uma pasta no caminho especificado:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

O caminho especificado é o caminho do mecanismo de exibição, que é o caminho relativo de raiz de páginas Razor.

## <a name="note-on-combining-authorized-and-anonymous-access"></a>Observe na combinação de autorização e acesso anônimo

É perfeitamente válido para especificar que uma pasta de páginas exigem autorização e especifique que uma página dentro da pasta permite acesso anônimo:

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

O inverso, no entanto, não é true. Você não pode declarar uma pasta de páginas para acesso anônimo e especificar uma página dentro de autorização:

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private") 
```

Exigir autorização na página privada não funcionará porque quando tanto o `AllowAnonymousFilter` e `AuthorizeFilter` filtros são aplicados para a página, o `AllowAnonymousFilter` wins e controla o acesso.

## <a name="additional-resources"></a>Recursos adicionais

* [Provedores de modelo personalizado de página e rota de Páginas Razor](xref:mvc/razor-pages/razor-pages-conventions)
* [PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) classe
