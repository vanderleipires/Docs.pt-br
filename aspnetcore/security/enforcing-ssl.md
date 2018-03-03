---
title: Impondo HTTPS em um aplicativo do ASP.NET Core
author: rick-anderson
description: "Mostra como exigir HTTPS/TLS em um núcleo de ASP.NET aplicativo web."
manager: wpickett
ms.author: riande
ms.date: 2/9/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/enforcing-ssl
ms.openlocfilehash: dc320faf0048200412f131ea816f33f29ac023e1
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/02/2018
---
# <a name="enforcing-https-in-an-aspnet-core-app"></a><span data-ttu-id="7ec07-103">Impondo HTTPS em um aplicativo do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7ec07-103">Enforcing HTTPS in an ASP.NET Core app</span></span>

<span data-ttu-id="7ec07-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7ec07-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="7ec07-105">Este documento demonstra como:</span><span class="sxs-lookup"><span data-stu-id="7ec07-105">This document shows how to:</span></span>

- <span data-ttu-id="7ec07-106">Exigir HTTPS para todas as solicitações.</span><span class="sxs-lookup"><span data-stu-id="7ec07-106">Require HTTPS for all requests.</span></span>
- <span data-ttu-id="7ec07-107">Redirecione todas as solicitações HTTP para HTTPS.</span><span class="sxs-lookup"><span data-stu-id="7ec07-107">Redirect all HTTP requests to HTTPS.</span></span>

> [!WARNING]
> <span data-ttu-id="7ec07-108">Fazer **não** usar `RequireHttpsAttribute` em APIs da Web que recebe informações confidenciais.</span><span class="sxs-lookup"><span data-stu-id="7ec07-108">Do **not** use `RequireHttpsAttribute` on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="7ec07-109">`RequireHttpsAttribute` usa códigos de status HTTP para redirecionar navegadores de HTTP para HTTPS.</span><span class="sxs-lookup"><span data-stu-id="7ec07-109">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="7ec07-110">Clientes de API podem não entender ou obedecer redirecionamentos de HTTP para HTTPS.</span><span class="sxs-lookup"><span data-stu-id="7ec07-110">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="7ec07-111">Esses clientes podem enviar informações sobre HTTP.</span><span class="sxs-lookup"><span data-stu-id="7ec07-111">Such clients may send information over HTTP.</span></span> <span data-ttu-id="7ec07-112">APIs da Web deverá:</span><span class="sxs-lookup"><span data-stu-id="7ec07-112">Web APIs should either:</span></span>
>
>* <span data-ttu-id="7ec07-113">Não escuta no HTTP.</span><span class="sxs-lookup"><span data-stu-id="7ec07-113">Not listen on HTTP.</span></span>
>* <span data-ttu-id="7ec07-114">Feche a conexão com o código de status 400 (solicitação incorreta) e não atender à solicitação.</span><span class="sxs-lookup"><span data-stu-id="7ec07-114">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

## <a name="require-https"></a><span data-ttu-id="7ec07-115">Exigir HTTPS</span><span class="sxs-lookup"><span data-stu-id="7ec07-115">Require HTTPS</span></span>

<span data-ttu-id="7ec07-116">O [RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute) é usada para exigir HTTPS.</span><span class="sxs-lookup"><span data-stu-id="7ec07-116">The [RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute) is used to require HTTPS.</span></span> <span data-ttu-id="7ec07-117">`[RequireHttpsAttribute]` pode decorar controladores ou métodos, ou podem ser aplicadas globalmente.</span><span class="sxs-lookup"><span data-stu-id="7ec07-117">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="7ec07-118">Para aplicar o atributo global, adicione o seguinte código ao `ConfigureServices` em `Startup`:</span><span class="sxs-lookup"><span data-stu-id="7ec07-118">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

<span data-ttu-id="7ec07-119">O código realçado anterior requer todas as solicitações usar `HTTPS`; portanto, as solicitações HTTP são ignoradas.</span><span class="sxs-lookup"><span data-stu-id="7ec07-119">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="7ec07-120">O seguinte código realçado redireciona todas as solicitações HTTP para HTTPS:</span><span class="sxs-lookup"><span data-stu-id="7ec07-120">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

<span data-ttu-id="7ec07-121">Para obter mais informações, consulte [Middleware de regravação de URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="7ec07-121">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>

<span data-ttu-id="7ec07-122">Exigir HTTPS globalmente (`options.Filters.Add(new RequireHttpsAttribute());`) é uma prática recomendada de segurança.</span><span class="sxs-lookup"><span data-stu-id="7ec07-122">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="7ec07-123">Aplicar o `[RequireHttps]` atributo para todas as páginas Razor/controladores não é considerado mais seguro que exijam HTTPS globalmente.</span><span class="sxs-lookup"><span data-stu-id="7ec07-123">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="7ec07-124">Você não pode garantir a `[RequireHttps]` atributo é aplicado quando forem adicionadas novos controladores e páginas Razor.</span><span class="sxs-lookup"><span data-stu-id="7ec07-124">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>