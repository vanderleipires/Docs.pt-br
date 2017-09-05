---
title: "Autorização baseada em modo de exibição"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 24ce40d8-9b83-4bae-9d4c-a66350fcc8f8
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/views
ms.openlocfilehash: 3b7fa6025d766da80ba92ee27af20bf9bfe0dcf4
ms.sourcegitcommit: 74e22e08e3b08cb576e5184d16f4af5656c13c0c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/25/2017
---
# <a name="view-based-authorization"></a><span data-ttu-id="7d368-103">Autorização baseada em modo de exibição</span><span class="sxs-lookup"><span data-stu-id="7d368-103">View Based Authorization</span></span>

<a name=security-authorization-views></a>

<span data-ttu-id="7d368-104">Geralmente um desenvolvedor deve mostrar, ocultar ou modificar uma interface do usuário com base na identidade do usuário atual.</span><span class="sxs-lookup"><span data-stu-id="7d368-104">Often a developer will want to show, hide or otherwise modify a UI based on the current user identity.</span></span> <span data-ttu-id="7d368-105">Você pode acessar o serviço de autorização em modos de exibição do MVC por meio de [injeção de dependência](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="7d368-105">You can access the authorization service within MVC views via [dependency injection](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection).</span></span> <span data-ttu-id="7d368-106">Injetar o serviço de autorização em um uso de exibição Razor o `@inject` diretiva, por exemplo `@inject IAuthorizationService AuthorizationService` (requer `@using Microsoft.AspNetCore.Authorization`).</span><span class="sxs-lookup"><span data-stu-id="7d368-106">To inject the authorization service into a Razor view use the `@inject` directive, for example `@inject IAuthorizationService AuthorizationService` (requires `@using Microsoft.AspNetCore.Authorization`).</span></span> <span data-ttu-id="7d368-107">Se você quiser que o serviço de autorização em cada exibição, em seguida, coloque o `@inject` diretiva no `_ViewImports.cshtml` do arquivo no `Views` directory.</span><span class="sxs-lookup"><span data-stu-id="7d368-107">If you want the authorization service in every view then place the `@inject` directive into the `_ViewImports.cshtml` file in the `Views` directory.</span></span> <span data-ttu-id="7d368-108">Para obter mais informações sobre injeção de dependência para modos de exibição, consulte [injeção de dependência para modos de exibição](../../mvc/views/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="7d368-108">For more information on dependency injection into views see [Dependency injection into views](../../mvc/views/dependency-injection.md).</span></span>

<span data-ttu-id="7d368-109">Depois que você tiver inserido o serviço de autorização usá-lo chamando o `AuthorizeAsync` método exatamente da mesma maneira que você deve verificar durante [autorização com base em recursos](resourcebased.md#security-authorization-resource-based-imperative).</span><span class="sxs-lookup"><span data-stu-id="7d368-109">Once you have injected the authorization service you use it by calling the `AuthorizeAsync` method in exactly the same way as you would check during [resource based authorization](resourcebased.md#security-authorization-resource-based-imperative).</span></span>

```csharp
@if (await AuthorizationService.AuthorizeAsync(User, "PolicyName"))
   {
       <p>This paragraph is displayed because you fulfilled PolicyName.</p>
   }
   ```

<span data-ttu-id="7d368-110">Em alguns casos, o recurso será o modelo de exibição, e você pode chamar `AuthorizeAsync` em exatamente da mesma maneira que você deve verificar durante [autorização com base em recursos](resourcebased.md#security-authorization-resource-based-imperative);</span><span class="sxs-lookup"><span data-stu-id="7d368-110">In some cases the resource will be your view model, and you can call `AuthorizeAsync` in exactly the same way as you would check during [resource based authorization](resourcebased.md#security-authorization-resource-based-imperative);</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7d368-111">ASP.NET Core 2. x</span><span class="sxs-lookup"><span data-stu-id="7d368-111">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
   @if ((await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit)).Succeeded)
   {
       <p><a class="btn btn-default" role="button"
           href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
   }
   ```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7d368-112">ASP.NET Core 1. x</span><span class="sxs-lookup"><span data-stu-id="7d368-112">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
   @if (await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit))
   {
       <p><a class="btn btn-default" role="button"
           href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
   }
   ```
---

<span data-ttu-id="7d368-113">Aqui você pode ver que o modelo é passado como a autorização de recursos deve levar em consideração.</span><span class="sxs-lookup"><span data-stu-id="7d368-113">Here you can see the model is passed as the resource authorization should take into consideration.</span></span>

>[!WARNING]
><span data-ttu-id="7d368-114">Não confie em Mostrar ou ocultar partes da sua interface do usuário, como seu único método de autorização.</span><span class="sxs-lookup"><span data-stu-id="7d368-114">Do not rely on showing or hiding parts of your UI as your only authorization method.</span></span> <span data-ttu-id="7d368-115">Ocultando uma interface de usuário elemento não significa que um usuário não pode acessá-lo.</span><span class="sxs-lookup"><span data-stu-id="7d368-115">Hiding a UI element does not mean a user cannot access it.</span></span> <span data-ttu-id="7d368-116">Você também deve autorizar o usuário dentro do seu código do controlador.</span><span class="sxs-lookup"><span data-stu-id="7d368-116">You must also authorize the user within your controller code.</span></span>
