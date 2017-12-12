---
title: "Imposição de SSL em um aplicativo do ASP.NET Core"
author: rick-anderson
description: "Mostra como exigir SSL em um núcleo de ASP.NET aplicativo web"
keywords: ASP.NET Core, SSL, HTTPS, RequireHttpsAttribute, IIS Express
ms.author: riande
manager: wpickett
ms.date: 07/19/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/enforcing-ssl
ms.openlocfilehash: 6f2755a606000717ca8a57f045b1ef613c7f14f6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
# <a name="enforcing-ssl-in-an-aspnet-core-app"></a><span data-ttu-id="7c2f4-104">Imposição de SSL em um aplicativo do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7c2f4-104">Enforcing SSL in an ASP.NET Core app</span></span>

<span data-ttu-id="7c2f4-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7c2f4-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="7c2f4-106">Este documento mostra como:</span><span class="sxs-lookup"><span data-stu-id="7c2f4-106">This document shows how to:</span></span>

- <span data-ttu-id="7c2f4-107">Exigir SSL para todas as solicitações (somente para solicitações HTTPS).</span><span class="sxs-lookup"><span data-stu-id="7c2f4-107">Require SSL for all requests (HTTPS requests only).</span></span>
- <span data-ttu-id="7c2f4-108">Redirecione todas as solicitações HTTP para HTTPS.</span><span class="sxs-lookup"><span data-stu-id="7c2f4-108">Redirect all HTTP requests to HTTPS.</span></span>

## <a name="require-ssl"></a><span data-ttu-id="7c2f4-109">Exigir SSL</span><span class="sxs-lookup"><span data-stu-id="7c2f4-109">Require SSL</span></span>

<span data-ttu-id="7c2f4-110">O [RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) é usada para exigir SSL.</span><span class="sxs-lookup"><span data-stu-id="7c2f4-110">The [RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require SSL.</span></span> <span data-ttu-id="7c2f4-111">Você pode decorar métodos com esse atributo ou controladores ou aplicá-lo globalmente, conforme mostrado abaixo:</span><span class="sxs-lookup"><span data-stu-id="7c2f4-111">You can decorate controllers or methods with this attribute or you can apply it globally as shown below:</span></span>

<span data-ttu-id="7c2f4-112">Adicione o seguinte código para `ConfigureServices` em `Startup`:</span><span class="sxs-lookup"><span data-stu-id="7c2f4-112">Add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-)]

<span data-ttu-id="7c2f4-113">O código acima requer que todas as solicitações usar `HTTPS`, portanto, as solicitações HTTP são ignoradas.</span><span class="sxs-lookup"><span data-stu-id="7c2f4-113">The highlighted code above requires all requests use `HTTPS`, therefore HTTP requests are ignored.</span></span> <span data-ttu-id="7c2f4-114">O seguinte código realçado redireciona todas as solicitações HTTP para HTTPS:</span><span class="sxs-lookup"><span data-stu-id="7c2f4-114">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-)]

<span data-ttu-id="7c2f4-115">Consulte [Middleware de regravação de URL](xref:fundamentals/url-rewriting) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="7c2f4-115">See [URL Rewriting Middleware](xref:fundamentals/url-rewriting) for more information.</span></span>

<span data-ttu-id="7c2f4-116">Exigir HTTPS globalmente (`options.Filters.Add(new RequireHttpsAttribute());`) é uma prática recomendada de segurança.</span><span class="sxs-lookup"><span data-stu-id="7c2f4-116">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="7c2f4-117">Aplicar o `[RequireHttps]` atributo ao controlador todos os não é considerado mais seguro que exijam HTTPS globalmente.</span><span class="sxs-lookup"><span data-stu-id="7c2f4-117">Applying the `[RequireHttps]` attribute to all controller is not considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="7c2f4-118">Você não pode garantir lembrará novos controladores adicionados ao seu aplicativo aplicar o `[RequireHttps]` atributo.</span><span class="sxs-lookup"><span data-stu-id="7c2f4-118">You can't guarantee new controllers added to your app will remember to apply the `[RequireHttps]` attribute.</span></span>
