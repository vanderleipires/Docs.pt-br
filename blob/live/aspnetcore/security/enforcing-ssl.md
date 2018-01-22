---
title: "Imposição de SSL em um aplicativo do ASP.NET Core"
author: rick-anderson
description: "Mostra como exigir SSL em um núcleo de ASP.NET aplicativo web"
ms.author: riande
manager: wpickett
ms.date: 07/19/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/enforcing-ssl
ms.openlocfilehash: 42d8767fda2d3f3545876f8ca18e0f6fbe6741b8
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/19/2018
---
# <a name="enforcing-ssl-in-an-aspnet-core-app"></a><span data-ttu-id="2ee73-103">Imposição de SSL em um aplicativo do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2ee73-103">Enforcing SSL in an ASP.NET Core app</span></span>

<span data-ttu-id="2ee73-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="2ee73-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="2ee73-105">Este documento mostra como:</span><span class="sxs-lookup"><span data-stu-id="2ee73-105">This document shows how to:</span></span>

- <span data-ttu-id="2ee73-106">Exigir SSL para todas as solicitações (somente para solicitações HTTPS).</span><span class="sxs-lookup"><span data-stu-id="2ee73-106">Require SSL for all requests (HTTPS requests only).</span></span>
- <span data-ttu-id="2ee73-107">Redirecione todas as solicitações HTTP para HTTPS.</span><span class="sxs-lookup"><span data-stu-id="2ee73-107">Redirect all HTTP requests to HTTPS.</span></span>

## <a name="require-ssl"></a><span data-ttu-id="2ee73-108">Exigir SSL</span><span class="sxs-lookup"><span data-stu-id="2ee73-108">Require SSL</span></span>

<span data-ttu-id="2ee73-109">O [RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) é usada para exigir SSL.</span><span class="sxs-lookup"><span data-stu-id="2ee73-109">The [RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require SSL.</span></span> <span data-ttu-id="2ee73-110">Você pode decorar métodos com esse atributo ou controladores ou aplicá-lo globalmente, conforme mostrado abaixo:</span><span class="sxs-lookup"><span data-stu-id="2ee73-110">You can decorate controllers or methods with this attribute or you can apply it globally as shown below:</span></span>

<span data-ttu-id="2ee73-111">Adicione o seguinte código para `ConfigureServices` em `Startup`:</span><span class="sxs-lookup"><span data-stu-id="2ee73-111">Add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-)]

<span data-ttu-id="2ee73-112">O código acima requer que todas as solicitações usar `HTTPS`, portanto, as solicitações HTTP são ignoradas.</span><span class="sxs-lookup"><span data-stu-id="2ee73-112">The highlighted code above requires all requests use `HTTPS`, therefore HTTP requests are ignored.</span></span> <span data-ttu-id="2ee73-113">O seguinte código realçado redireciona todas as solicitações HTTP para HTTPS:</span><span class="sxs-lookup"><span data-stu-id="2ee73-113">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-)]

<span data-ttu-id="2ee73-114">Consulte [Middleware de regravação de URL](xref:fundamentals/url-rewriting) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="2ee73-114">See [URL Rewriting Middleware](xref:fundamentals/url-rewriting) for more information.</span></span>

<span data-ttu-id="2ee73-115">Exigir HTTPS globalmente (`options.Filters.Add(new RequireHttpsAttribute());`) é uma prática recomendada de segurança.</span><span class="sxs-lookup"><span data-stu-id="2ee73-115">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="2ee73-116">Aplicar o `[RequireHttps]` atributo ao controlador todos os não é considerado mais seguro que exijam HTTPS globalmente.</span><span class="sxs-lookup"><span data-stu-id="2ee73-116">Applying the `[RequireHttps]` attribute to all controller is not considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="2ee73-117">Você não pode garantir lembrará novos controladores adicionados ao seu aplicativo aplicar o `[RequireHttps]` atributo.</span><span class="sxs-lookup"><span data-stu-id="2ee73-117">You can't guarantee new controllers added to your app will remember to apply the `[RequireHttps]` attribute.</span></span>
