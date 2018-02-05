---
title: "Imposição de SSL em um aplicativo do ASP.NET Core"
author: rick-anderson
description: "Mostra como exigir SSL em um núcleo de ASP.NET aplicativo web"
manager: wpickett
ms.author: riande
ms.date: 07/19/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/enforcing-ssl
ms.openlocfilehash: 3b72cddb7a240ad6d6e1427796e9bb4f7003a3f7
ms.sourcegitcommit: 7a87d66cf1d01febe6635c7306f2f679434901d1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/03/2018
---
# <a name="enforcing-ssl-in-an-aspnet-core-app"></a><span data-ttu-id="7d9da-103">Imposição de SSL em um aplicativo do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7d9da-103">Enforcing SSL in an ASP.NET Core app</span></span>

<span data-ttu-id="7d9da-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7d9da-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="7d9da-105">Este documento mostra como:</span><span class="sxs-lookup"><span data-stu-id="7d9da-105">This document shows how to:</span></span>

- <span data-ttu-id="7d9da-106">Exigir SSL para todas as solicitações (somente para solicitações HTTPS).</span><span class="sxs-lookup"><span data-stu-id="7d9da-106">Require SSL for all requests (HTTPS requests only).</span></span>
- <span data-ttu-id="7d9da-107">Redirecione todas as solicitações HTTP para HTTPS.</span><span class="sxs-lookup"><span data-stu-id="7d9da-107">Redirect all HTTP requests to HTTPS.</span></span>

## <a name="require-ssl"></a><span data-ttu-id="7d9da-108">Exigir SSL</span><span class="sxs-lookup"><span data-stu-id="7d9da-108">Require SSL</span></span>

<span data-ttu-id="7d9da-109">O [RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) é usada para exigir SSL.</span><span class="sxs-lookup"><span data-stu-id="7d9da-109">The [RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require SSL.</span></span> <span data-ttu-id="7d9da-110">Você pode decorar métodos com esse atributo ou controladores ou aplicá-lo globalmente, conforme mostrado abaixo:</span><span class="sxs-lookup"><span data-stu-id="7d9da-110">You can decorate controllers or methods with this attribute or you can apply it globally as shown below:</span></span>

<span data-ttu-id="7d9da-111">Adicione o seguinte código para `ConfigureServices` em `Startup`:</span><span class="sxs-lookup"><span data-stu-id="7d9da-111">Add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

<span data-ttu-id="7d9da-112">O código acima requer que todas as solicitações usar `HTTPS`, portanto, as solicitações HTTP são ignoradas.</span><span class="sxs-lookup"><span data-stu-id="7d9da-112">The highlighted code above requires all requests use `HTTPS`, therefore HTTP requests are ignored.</span></span> <span data-ttu-id="7d9da-113">O seguinte código realçado redireciona todas as solicitações HTTP para HTTPS:</span><span class="sxs-lookup"><span data-stu-id="7d9da-113">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

<span data-ttu-id="7d9da-114">Consulte [Middleware de regravação de URL](xref:fundamentals/url-rewriting) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="7d9da-114">See [URL Rewriting Middleware](xref:fundamentals/url-rewriting) for more information.</span></span>

<span data-ttu-id="7d9da-115">Exigir HTTPS globalmente (`options.Filters.Add(new RequireHttpsAttribute());`) é uma prática recomendada de segurança.</span><span class="sxs-lookup"><span data-stu-id="7d9da-115">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="7d9da-116">Aplicar o `[RequireHttps]` atributo para todos os do controlador não é considerado mais seguro que exijam HTTPS globalmente.</span><span class="sxs-lookup"><span data-stu-id="7d9da-116">Applying the `[RequireHttps]` attribute to all controller isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="7d9da-117">Você não pode garantir lembrará novos controladores adicionados ao seu aplicativo aplicar o `[RequireHttps]` atributo.</span><span class="sxs-lookup"><span data-stu-id="7d9da-117">You can't guarantee new controllers added to your app will remember to apply the `[RequireHttps]` attribute.</span></span>
