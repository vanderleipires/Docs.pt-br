---
title: "Autorização baseada em modo de exibição no ASP.NET MVC de núcleo"
author: rick-anderson
description: "Este documento demonstra como injetar e utilizar o serviço de autorização dentro de um modo de exibição Razor do ASP.NET Core."
keywords: "ASP.NET Core, autorização de Razor de autorização, IAuthorizationService,"
ms.author: riande
manager: wpickett
ms.date: 10/30/2017
ms.topic: article
ms.assetid: 24ce40d8-9b83-4bae-9d4c-a66350fcc8f8
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/views
ms.openlocfilehash: 756431f398c29376ab0ecd6c4f4d1db4f8022b0b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
# <a name="view-based-authorization"></a><span data-ttu-id="a65c8-104">Autorização baseada em modo de exibição</span><span class="sxs-lookup"><span data-stu-id="a65c8-104">View-based authorization</span></span>

<span data-ttu-id="a65c8-105">Um desenvolvedor quer geralmente Mostrar, ocultar ou modificar uma interface do usuário com base na identidade do usuário atual.</span><span class="sxs-lookup"><span data-stu-id="a65c8-105">A developer often wants to show, hide, or otherwise modify a UI based on the current user identity.</span></span> <span data-ttu-id="a65c8-106">Você pode acessar o serviço de autorização em modos de exibição do MVC por meio de [injeção de dependência](xref:fundamentals/dependency-injection#fundamentals-dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="a65c8-106">You can access the authorization service within MVC views via [dependency injection](xref:fundamentals/dependency-injection#fundamentals-dependency-injection).</span></span> <span data-ttu-id="a65c8-107">Para injetar o serviço de autorização em um modo de exibição Razor, use o `@inject` diretiva:</span><span class="sxs-lookup"><span data-stu-id="a65c8-107">To inject the authorization service into a Razor view, use the `@inject` directive:</span></span>

```cshtml
@using Microsoft.AspNetCore.Authorization
@inject IAuthorizationService AuthorizationService
```

<span data-ttu-id="a65c8-108">Se você deseja que o serviço de autorização em cada exibição, coloque o `@inject` diretiva para o *viewimports. cshtml* arquivo do *exibições* diretório.</span><span class="sxs-lookup"><span data-stu-id="a65c8-108">If you want the authorization service in every view, place the `@inject` directive into the *_ViewImports.cshtml* file of the *Views* directory.</span></span> <span data-ttu-id="a65c8-109">Para obter mais informações, consulte [injeção de dependência para modos de exibição](xref:mvc/views/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="a65c8-109">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span>

<span data-ttu-id="a65c8-110">Usar o serviço de autorização injetado para invocar `AuthorizeAsync` exatamente da mesma forma que você deve verificar durante [autorização baseada em recursos](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span><span class="sxs-lookup"><span data-stu-id="a65c8-110">Use the injected authorization service to invoke `AuthorizeAsync` in exactly the same way you would check during [resource-based authorization](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a65c8-111">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a65c8-111">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, "PolicyName")).Succeeded)
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a65c8-112">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a65c8-112">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, "PolicyName"))
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

---

<span data-ttu-id="a65c8-113">Em alguns casos, o recurso será o modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="a65c8-113">In some cases, the resource will be your view model.</span></span> <span data-ttu-id="a65c8-114">Invocar `AuthorizeAsync` exatamente da mesma forma que você deve verificar durante [autorização baseada em recursos](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span><span class="sxs-lookup"><span data-stu-id="a65c8-114">Invoke `AuthorizeAsync` in exactly the same way you would check during [resource-based authorization](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a65c8-115">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a65c8-115">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit)).Succeeded)
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a65c8-116">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a65c8-116">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit))
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

---

<span data-ttu-id="a65c8-117">No código anterior, o modelo é passado como um recurso de que avaliação de política deve levar em consideração.</span><span class="sxs-lookup"><span data-stu-id="a65c8-117">In the preceding code, the model is passed as a resource the policy evaluation should take into consideration.</span></span>

> [!WARNING]
> <span data-ttu-id="a65c8-118">Não confie na alternância de visibilidade de elementos de interface do usuário do aplicativo como a verificação de autorização exclusiva.</span><span class="sxs-lookup"><span data-stu-id="a65c8-118">Don't rely on toggling visibility of your app's UI elements as the sole authorization check.</span></span> <span data-ttu-id="a65c8-119">Ocultar um elemento de interface do usuário pode completamente impede o acesso a sua ação de controlador associado.</span><span class="sxs-lookup"><span data-stu-id="a65c8-119">Hiding a UI element may not completely prevent access to its associated controller action.</span></span> <span data-ttu-id="a65c8-120">Por exemplo, considere o botão no trecho de código anterior.</span><span class="sxs-lookup"><span data-stu-id="a65c8-120">For example, consider the button in the preceding code snippet.</span></span> <span data-ttu-id="a65c8-121">Um usuário pode invocar o `Edit` URL do método de ação se saiba o recurso relativo é */Document/Edit/1*.</span><span class="sxs-lookup"><span data-stu-id="a65c8-121">A user can invoke the `Edit` action method if he or she knows the relative resource URL is */Document/Edit/1*.</span></span> <span data-ttu-id="a65c8-122">Por esse motivo, o `Edit` método de ação deve executar sua própria verificação de autorização.</span><span class="sxs-lookup"><span data-stu-id="a65c8-122">For this reason, the `Edit` action method should perform its own authorization check.</span></span>
