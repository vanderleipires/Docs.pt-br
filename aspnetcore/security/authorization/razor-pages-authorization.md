---
title: Convenções de autorização de páginas do Razor no ASP.NET Core
author: guardrex
description: Saiba como controlar o acesso a páginas com as convenções de autorizam usuários e permitir que usuários anônimos acessem páginas ou pastas de páginas.
ms.author: riande
ms.custom: mvc
ms.date: 10/27/2017
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: 675dc8aa4bf00bb21981cc892a09a4acd0d53c15
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207258"
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a>Convenções de autorização de páginas do Razor no ASP.NET Core

Por [Luke Latham](https://github.com/guardrex)

Uma maneira de controlar o acesso em seu aplicativo de páginas do Razor é usar as convenções de autorização na inicialização. Essas convenções permitem que você autorizar usuários e permitir que usuários anônimos acessem páginas individuais ou pastas de páginas. Aplicam as convenções descritas neste tópico automaticamente [filtros de autorização](xref:mvc/controllers/filters#authorization-filters) para controlar o acesso.

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([como baixar](xref:index#how-to-download-a-sample))

O aplicativo de exemplo usa [autenticação de Cookie sem o ASP.NET Core Identity](xref:security/authentication/cookie). A conta de usuário para o usuário hipotético, Maria Rodriguez, é codificados no aplicativo. Use o nome de usuário de Email "maria.rodriguez@contoso.com" e nenhuma senha para a entrada do usuário. O usuário é autenticado na `AuthenticateUser` método na *Pages/Account/Login.cshtml.cs* arquivo. Em um exemplo do mundo real, o usuário deve ser autenticado em relação a um banco de dados. Para usar o ASP.NET Core Identity, siga as diretrizes a [Introdução à identidade no ASP.NET Core](xref:security/authentication/identity) tópico. Os conceitos e os exemplos mostrados neste tópico se aplicam igualmente a aplicativos que usam o ASP.NET Core Identity.

## <a name="require-authorization-to-access-a-page"></a>Exigir autorização para acessar uma página

Use o [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) convenção via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) para adicionar um [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) para a página no caminho especificado:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

O caminho especificado é o caminho de mecanismo de exibição, que é o caminho relativo de raiz de páginas do Razor sem uma extensão e que contém apenas barras "/".

Uma [AuthorizePage sobrecarga](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) estará disponível se você precisa especificar uma política de autorização.

::: moniker range=">= aspnetcore-2.1"

> [!NOTE]
> Uma `AuthorizeFilter` pode ser aplicado a uma classe de modelo de página com o `[Authorize]` atributo de filtro. Para obter mais informações, consulte [atributo de filtro Authorize](xref:razor-pages/filter#authorize-filter-attribute).

::: moniker-end

## <a name="require-authorization-to-access-a-folder-of-pages"></a>Exigir autorização para acessar uma pasta de páginas

Use o [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) convenção via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) para adicionar um [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) para todas as páginas em uma pasta no caminho especificado:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

O caminho especificado é o caminho de mecanismo de exibição, que é o caminho relativo de raiz de páginas do Razor.

Uma [AuthorizeFolder sobrecarga](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) estará disponível se você precisa especificar uma política de autorização.

::: moniker range=">= aspnetcore-2.1"

## <a name="require-authorization-to-access-an-area-page"></a>Exigir autorização para acessar uma página de área

Use o [AuthorizeAreaPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareapage) convenção via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) para adicionar um [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) para a página de área no caminho especificado:

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

O nome da página é o caminho do arquivo sem uma extensão relativo ao diretório raiz páginas para a área especificada. Por exemplo, o nome da página para o arquivo *Areas/Identity/Pages/Manage/Accounts.cshtml* é *contas/gerenciar/*.

Uma [AuthorizeAreaPage sobrecarga](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareapage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeAreaPage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_System_String_) estará disponível se você precisa especificar uma política de autorização.

## <a name="require-authorization-to-access-a-folder-of-areas"></a>Exigir autorização para acessar uma pasta de áreas

Use o [AuthorizeAreaFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareafolder) convenção via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) para adicionar um [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) para todas as áreas em uma pasta no caminho especificado:

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

O caminho da pasta é o caminho da pasta, relativo ao diretório raiz páginas para a área especificada. Por exemplo, o caminho da pasta para os arquivos sob *áreas/identidade/páginas/gerenciar/* é */gerenciar*.

Uma [AuthorizeAreaFolder sobrecarga](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareafolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeAreaFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_System_String_) estará disponível se você precisa especificar uma política de autorização.

::: moniker-end

## <a name="allow-anonymous-access-to-a-page"></a>Permitir o acesso anônimo para uma página

Use o [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) convenção via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) para adicionar um [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) para uma página no caminho especificado:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

O caminho especificado é o caminho de mecanismo de exibição, que é o caminho relativo de raiz de páginas do Razor sem uma extensão e que contém apenas barras "/".

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a>Permitir o acesso anônimo para uma pasta de páginas

Use o [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) convenção via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) para adicionar um [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) para todas as páginas em uma pasta no caminho especificado:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

O caminho especificado é o caminho de mecanismo de exibição, que é o caminho relativo de raiz de páginas do Razor.

## <a name="note-on-combining-authorized-and-anonymous-access"></a>Observação sobre combinação autorizada e acesso anônimo

É perfeitamente válido para especificar que uma pasta de páginas exigem autorização e especificar uma página dentro da pasta permite o acesso anônimo:

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

O inverso, no entanto, não é verdadeiro. Você não pode declarar uma pasta de páginas para acesso anônimo e especificar uma página dentro de autorização:

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private") 
```

Exigir autorização na página privada não funcionará porque quando tanto o `AllowAnonymousFilter` e `AuthorizeFilter` filtros são aplicados para a página, o `AllowAnonymousFilter` wins e controla o acesso.

## <a name="additional-resources"></a>Recursos adicionais

* [Provedores de modelo personalizado de página e rota de Páginas Razor](xref:razor-pages/razor-pages-conventions)
* [PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) classe
