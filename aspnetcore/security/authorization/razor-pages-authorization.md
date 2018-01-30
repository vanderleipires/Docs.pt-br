---
title: "Convenções de autorização de páginas Razor do ASP.NET Core"
author: guardrex
description: "Aprenda a controlar o acesso a páginas com as convenções na inicialização que autorizam usuários e permitir que usuários anônimos acessem páginas individuais ou pastas de páginas."
manager: wpickett
ms.author: riande
ms.date: 10/27/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: 2bad6e1cc654b972206af03f99160628f81e026f
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2018
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a>Convenções de autorização de páginas Razor do ASP.NET Core

Por [Luke Latham](https://github.com/guardrex)

Uma maneira de controlar o acesso em seu aplicativo de páginas Razor é usar as convenções de autorização na inicialização. As seguintes convenções permitem que você autorizar os usuários e permitir que usuários anônimos acessem páginas individuais ou pastas de páginas. Aplicam as convenções descritas neste tópico automaticamente [filtros de autorização](xref:mvc/controllers/filters#authorization-filters) para controlar o acesso.

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

## <a name="require-authorization-to-access-a-page"></a>Exigir autorização para acessar uma página

Use o [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) convenção via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) para adicionar um [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) para a página no caminho especificado:

[!code-csharp[Main](razor-pages-authorization/sample/Startup.cs?name=snippet1&highlight=2,4)]

O caminho especificado é o caminho do mecanismo de exibição, que é o caminho relativo do Razor páginas raiz sem uma extensão e contendo apenas barras invertidas.

Um [AuthorizePage sobrecarga](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) estará disponível se você precisa especificar uma política de autorização.

## <a name="require-authorization-to-access-a-folder-of-pages"></a>Exigir autorização para acessar uma pasta de páginas

Use o [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) convenção via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) para adicionar um [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) para todas as páginas em uma pasta no caminho especificado:

[!code-csharp[Main](razor-pages-authorization/sample/Startup.cs?name=snippet1&highlight=2,5)]

O caminho especificado é o caminho do mecanismo de exibição, que é o caminho relativo de raiz de páginas Razor.

Um [AuthorizeFolder sobrecarga](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) estará disponível se você precisa especificar uma política de autorização.

## <a name="allow-anonymous-access-to-a-page"></a>Permitir acesso anônimo a uma página

Use o [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) convenção via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) para adicionar um [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) para uma página no caminho especificado:

[!code-csharp[Main](razor-pages-authorization/sample/Startup.cs?name=snippet1&highlight=2,6)]

O caminho especificado é o caminho do mecanismo de exibição, que é o caminho relativo do Razor páginas raiz sem uma extensão e contendo apenas barras invertidas.

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a>Permitir o acesso anônimo para uma pasta de páginas

Use o [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) convenção via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) para adicionar um [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) para todas as páginas em uma pasta no caminho especificado:

[!code-csharp[Main](razor-pages-authorization/sample/Startup.cs?name=snippet1&highlight=2,7)]

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

## <a name="see-also"></a>Consulte também

* [Provedores de modelo personalizado de página e rota de Páginas Razor](xref:mvc/razor-pages/razor-pages-convention-features)
* [PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) classe
