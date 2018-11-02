---
title: Plataformas com suporte do SignalR do ASP.NET Core
author: tdykstra
description: Saiba mais sobre as plataformas com suporte para o SignalR do ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 10/31/2018
uid: signalr/supported-platforms
ms.openlocfilehash: 773f6c020dbb2982911e177b55855473c750d52a
ms.sourcegitcommit: fc2486ddbeb15ab4969168d99b3fe0fbe91e8661
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/01/2018
ms.locfileid: "50758174"
---
# <a name="aspnet-core-signalr-supported-platforms"></a><span data-ttu-id="a9330-103">Plataformas com suporte do SignalR do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a9330-103">ASP.NET Core SignalR supported platforms</span></span>

## <a name="server-system-requirements"></a><span data-ttu-id="a9330-104">Requisitos de sistema do servidor</span><span class="sxs-lookup"><span data-stu-id="a9330-104">Server system requirements</span></span>

<span data-ttu-id="a9330-105">SignalR para ASP.NET Core dá suporte a qualquer plataforma de servidor que dá suporte ao ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a9330-105">SignalR for ASP.NET Core supports any server platform that ASP.NET Core supports.</span></span>

## <a name="javascript-client"></a><span data-ttu-id="a9330-106">Cliente JavaScript</span><span class="sxs-lookup"><span data-stu-id="a9330-106">JavaScript client</span></span>

<span data-ttu-id="a9330-107">O [cliente JavaScript](https://www.npmjs.com/package/@aspnet/signalr) é executado no NodeJS 8 e versões posteriores e os seguintes navegadores:</span><span class="sxs-lookup"><span data-stu-id="a9330-107">The [JavaScript client](https://www.npmjs.com/package/@aspnet/signalr) runs on NodeJS 8 and later versions and the following browsers:</span></span>

| <span data-ttu-id="a9330-108">Navegador</span><span class="sxs-lookup"><span data-stu-id="a9330-108">Browser</span></span>                         | <span data-ttu-id="a9330-109">Versão</span><span class="sxs-lookup"><span data-stu-id="a9330-109">Version</span></span> |
| ------------------------------- | ------- |
| <span data-ttu-id="a9330-110">Microsoft Edge</span><span class="sxs-lookup"><span data-stu-id="a9330-110">Microsoft Edge</span></span>                  | <span data-ttu-id="a9330-111">atual</span><span class="sxs-lookup"><span data-stu-id="a9330-111">current</span></span> |
| <span data-ttu-id="a9330-112">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="a9330-112">Mozilla Firefox</span></span>                 | <span data-ttu-id="a9330-113">atual</span><span class="sxs-lookup"><span data-stu-id="a9330-113">current</span></span> |
| <span data-ttu-id="a9330-114">Google Chrome; inclui o Android</span><span class="sxs-lookup"><span data-stu-id="a9330-114">Google Chrome; includes Android</span></span> | <span data-ttu-id="a9330-115">atual</span><span class="sxs-lookup"><span data-stu-id="a9330-115">current</span></span> |
| <span data-ttu-id="a9330-116">Safari; inclui o iOS</span><span class="sxs-lookup"><span data-stu-id="a9330-116">Safari; includes iOS</span></span>            | <span data-ttu-id="a9330-117">atual</span><span class="sxs-lookup"><span data-stu-id="a9330-117">current</span></span> |
| <span data-ttu-id="a9330-118">Microsoft Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="a9330-118">Microsoft Internet Explorer</span></span>     | <span data-ttu-id="a9330-119">11</span><span class="sxs-lookup"><span data-stu-id="a9330-119">11</span></span>      |
 
## <a name="net-client"></a><span data-ttu-id="a9330-120">Cliente .NET</span><span class="sxs-lookup"><span data-stu-id="a9330-120">.NET client</span></span>

<span data-ttu-id="a9330-121">O [cliente .NET](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) é executado em qualquer plataforma de servidor com suporte pelo ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a9330-121">The [.NET client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) runs on any server platform supported by ASP.NET Core.</span></span>

<span data-ttu-id="a9330-122">Se o servidor executa o IIS, o transporte de WebSockets requer o IIS 8.0 ou superior no Windows Server 2012 ou superior.</span><span class="sxs-lookup"><span data-stu-id="a9330-122">If the server runs IIS, the WebSockets transport requires IIS 8.0 or higher on Windows Server 2012 or higher.</span></span> <span data-ttu-id="a9330-123">Outros transportes têm suporte em todas as plataformas.</span><span class="sxs-lookup"><span data-stu-id="a9330-123">Other transports are supported on all platforms.</span></span>

## <a name="java-client"></a><span data-ttu-id="a9330-124">Cliente de Java</span><span class="sxs-lookup"><span data-stu-id="a9330-124">Java client</span></span>

<span data-ttu-id="a9330-125">O [cliente Java](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) dá suporte a Java 8 e versões posteriores.</span><span class="sxs-lookup"><span data-stu-id="a9330-125">The [Java client](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) supports Java 8 and later versions.</span></span>

## <a name="unsupported-clients"></a><span data-ttu-id="a9330-126">Não há suporte para clientes</span><span class="sxs-lookup"><span data-stu-id="a9330-126">Unsupported clients</span></span>

<span data-ttu-id="a9330-127">Os clientes a seguir estão disponíveis, mas são experimentais ou não oficial.</span><span class="sxs-lookup"><span data-stu-id="a9330-127">The following clients are available but are experimental or unofficial.</span></span> <span data-ttu-id="a9330-128">Eles não têm suporte no momento e nunca podem ser.</span><span class="sxs-lookup"><span data-stu-id="a9330-128">They aren't currently supported and may never be.</span></span>

* [<span data-ttu-id="a9330-129">Cliente C++</span><span class="sxs-lookup"><span data-stu-id="a9330-129">C++ client</span></span>](https://github.com/aspnet/SignalR/tree/master/clients/cpp)

* [<span data-ttu-id="a9330-130">Cliente SWIFT</span><span class="sxs-lookup"><span data-stu-id="a9330-130">Swift client</span></span>](https://github.com/moozzyk/SignalR-Client-Swift)
