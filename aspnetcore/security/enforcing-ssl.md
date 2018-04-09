---
title: Impor HTTPS do núcleo do ASP.NET
author: rick-anderson
description: Mostra como exigir HTTPS/TLS em um núcleo de ASP.NET aplicativo web.
manager: wpickett
ms.author: riande
ms.date: 2/9/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/enforcing-ssl
ms.openlocfilehash: 2ebb975e1ea17698cee13ca00d3f5df4a5135e38
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/22/2018
---
# <a name="enforce-https-in-an-aspnet-core"></a><span data-ttu-id="a597d-103">Impor HTTPS do núcleo do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="a597d-103">Enforce HTTPS in an ASP.NET Core</span></span>

<span data-ttu-id="a597d-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a597d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a597d-105">Este documento demonstra como:</span><span class="sxs-lookup"><span data-stu-id="a597d-105">This document shows how to:</span></span>

- <span data-ttu-id="a597d-106">Exigir HTTPS para todas as solicitações.</span><span class="sxs-lookup"><span data-stu-id="a597d-106">Require HTTPS for all requests.</span></span>
- <span data-ttu-id="a597d-107">Redirecione todas as solicitações HTTP para HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a597d-107">Redirect all HTTP requests to HTTPS.</span></span>

> [!WARNING]
> <span data-ttu-id="a597d-108">Fazer **não** usar `RequireHttpsAttribute` em APIs da Web que recebe informações confidenciais.</span><span class="sxs-lookup"><span data-stu-id="a597d-108">Do **not** use `RequireHttpsAttribute` on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="a597d-109">`RequireHttpsAttribute` usa códigos de status HTTP para redirecionar navegadores de HTTP para HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a597d-109">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="a597d-110">Clientes de API podem não entender ou obedecer redirecionamentos de HTTP para HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a597d-110">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="a597d-111">Esses clientes podem enviar informações sobre HTTP.</span><span class="sxs-lookup"><span data-stu-id="a597d-111">Such clients may send information over HTTP.</span></span> <span data-ttu-id="a597d-112">APIs da Web deverá:</span><span class="sxs-lookup"><span data-stu-id="a597d-112">Web APIs should either:</span></span>
>
>* <span data-ttu-id="a597d-113">Não escuta no HTTP.</span><span class="sxs-lookup"><span data-stu-id="a597d-113">Not listen on HTTP.</span></span>
>* <span data-ttu-id="a597d-114">Feche a conexão com o código de status 400 (solicitação incorreta) e não atender à solicitação.</span><span class="sxs-lookup"><span data-stu-id="a597d-114">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

## <a name="require-https"></a><span data-ttu-id="a597d-115">Exigir HTTPS</span><span class="sxs-lookup"><span data-stu-id="a597d-115">Require HTTPS</span></span>

<span data-ttu-id="a597d-116">O [RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute) é usada para exigir HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a597d-116">The [RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute) is used to require HTTPS.</span></span> <span data-ttu-id="a597d-117">`[RequireHttpsAttribute]` pode decorar controladores ou métodos, ou podem ser aplicadas globalmente.</span><span class="sxs-lookup"><span data-stu-id="a597d-117">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="a597d-118">Para aplicar o atributo global, adicione o seguinte código ao `ConfigureServices` em `Startup`:</span><span class="sxs-lookup"><span data-stu-id="a597d-118">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

<span data-ttu-id="a597d-119">O código realçado anterior requer todas as solicitações usar `HTTPS`; portanto, as solicitações HTTP são ignoradas.</span><span class="sxs-lookup"><span data-stu-id="a597d-119">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="a597d-120">O seguinte código realçado redireciona todas as solicitações HTTP para HTTPS:</span><span class="sxs-lookup"><span data-stu-id="a597d-120">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

<span data-ttu-id="a597d-121">Para obter mais informações, consulte [Middleware de regravação de URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="a597d-121">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>

<span data-ttu-id="a597d-122">Exigir HTTPS globalmente (`options.Filters.Add(new RequireHttpsAttribute());`) é uma prática recomendada de segurança.</span><span class="sxs-lookup"><span data-stu-id="a597d-122">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="a597d-123">Aplicar o `[RequireHttps]` atributo para todas as páginas Razor/controladores não é considerado mais seguro que exijam HTTPS globalmente.</span><span class="sxs-lookup"><span data-stu-id="a597d-123">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="a597d-124">Você não pode garantir a `[RequireHttps]` atributo é aplicado quando forem adicionadas novos controladores e páginas Razor.</span><span class="sxs-lookup"><span data-stu-id="a597d-124">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>
