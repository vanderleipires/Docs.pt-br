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
ms.openlocfilehash: 35554939bd574b2826053004ed437bee46154c2b
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2017
---
# <a name="enforcing-ssl-in-an-aspnet-core-app"></a><span data-ttu-id="6f0cf-104">Imposição de SSL em um aplicativo do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6f0cf-104">Enforcing SSL in an ASP.NET Core app</span></span>

<span data-ttu-id="6f0cf-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="6f0cf-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="6f0cf-106">Este documento mostra como:</span><span class="sxs-lookup"><span data-stu-id="6f0cf-106">This document shows how to:</span></span>

- <span data-ttu-id="6f0cf-107">Exigir SSL para todas as solicitações (somente para solicitações HTTPS).</span><span class="sxs-lookup"><span data-stu-id="6f0cf-107">Require SSL for all requests (HTTPS requests only).</span></span>
- <span data-ttu-id="6f0cf-108">Redirecione todas as solicitações HTTP para HTTPS.</span><span class="sxs-lookup"><span data-stu-id="6f0cf-108">Redirect all HTTP requests to HTTPS.</span></span>

## <a name="require-ssl"></a><span data-ttu-id="6f0cf-109">Exigir SSL</span><span class="sxs-lookup"><span data-stu-id="6f0cf-109">Require SSL</span></span>

<span data-ttu-id="6f0cf-110">O [RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) é usada para exigir SSL.</span><span class="sxs-lookup"><span data-stu-id="6f0cf-110">The [RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require SSL.</span></span> <span data-ttu-id="6f0cf-111">Você pode decorar métodos com esse atributo ou controladores ou aplicá-lo globalmente, conforme mostrado abaixo:</span><span class="sxs-lookup"><span data-stu-id="6f0cf-111">You can decorate controllers or methods with this attribute or you can apply it globally as shown below:</span></span>

<span data-ttu-id="6f0cf-112">Adicione o seguinte código para `ConfigureServices` em `Startup`:</span><span class="sxs-lookup"><span data-stu-id="6f0cf-112">Add the following code to `ConfigureServices` in `Startup`:</span></span>

<span data-ttu-id="6f0cf-113">[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-)]</span><span class="sxs-lookup"><span data-stu-id="6f0cf-113">[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-)]</span></span>

<span data-ttu-id="6f0cf-114">O código acima requer que todas as solicitações usar `HTTPS`, portanto, as solicitações HTTP são ignoradas.</span><span class="sxs-lookup"><span data-stu-id="6f0cf-114">The highlighted code above requires all requests use `HTTPS`, therefore HTTP requests are ignored.</span></span> <span data-ttu-id="6f0cf-115">O seguinte código realçado redireciona todas as solicitações HTTP para HTTPS:</span><span class="sxs-lookup"><span data-stu-id="6f0cf-115">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

<span data-ttu-id="6f0cf-116">[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-)]</span><span class="sxs-lookup"><span data-stu-id="6f0cf-116">[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-)]</span></span>

<span data-ttu-id="6f0cf-117">Consulte [Middleware de regravação de URL](xref:fundamentals/url-rewriting) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="6f0cf-117">See [URL Rewriting Middleware](xref:fundamentals/url-rewriting) for more information.</span></span>

<span data-ttu-id="6f0cf-118">Exigir HTTPS globalmente (`options.Filters.Add(new RequireHttpsAttribute());`) é uma prática recomendada de segurança.</span><span class="sxs-lookup"><span data-stu-id="6f0cf-118">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="6f0cf-119">Aplicar o `[RequireHttps]` atributo ao controlador todos os não é considerado mais seguro que exijam HTTPS globalmente.</span><span class="sxs-lookup"><span data-stu-id="6f0cf-119">Applying the `[RequireHttps]` attribute to all controller is not considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="6f0cf-120">Você não pode garantir lembrará novos controladores adicionados ao seu aplicativo aplicar o `[RequireHttps]` atributo.</span><span class="sxs-lookup"><span data-stu-id="6f0cf-120">You can't guarantee new controllers added to your app will remember to apply the `[RequireHttps]` attribute.</span></span>

## <a name="set-up-iis-express-for-sslhttps"></a><span data-ttu-id="6f0cf-121">Configurar o IIS Express para HTTPS/SSL</span><span class="sxs-lookup"><span data-stu-id="6f0cf-121">Set up IIS Express for SSL/HTTPS</span></span>

<span data-ttu-id="6f0cf-122">Consulte [configurar o HTTPS para o desenvolvimento em ASP.NET Core](xref:security/https#iisxpress).</span><span class="sxs-lookup"><span data-stu-id="6f0cf-122">See [Setting up HTTPS for development in ASP.NET Core](xref:security/https#iisxpress).</span></span>
