---
title: Plataformas com suporte do SignalR do ASP.NET Core
author: tdykstra
description: Saiba mais sobre as plataformas com suporte para o SignalR do ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/12/2018
uid: signalr/supported-platforms
ms.openlocfilehash: be3d4d0049395fb2499bd0b4aac126e953ce7910
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861713"
---
# <a name="aspnet-core-signalr-supported-platforms"></a><span data-ttu-id="1248f-103">Plataformas com suporte do SignalR do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1248f-103">ASP.NET Core SignalR supported platforms</span></span>

## <a name="server-system-requirements"></a><span data-ttu-id="1248f-104">Requisitos de sistema do servidor</span><span class="sxs-lookup"><span data-stu-id="1248f-104">Server system requirements</span></span>

<span data-ttu-id="1248f-105">SignalR para ASP.NET Core dá suporte a qualquer plataforma de servidor que dá suporte ao ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1248f-105">SignalR for ASP.NET Core supports any server platform that ASP.NET Core supports.</span></span>

## <a name="javascript-client"></a><span data-ttu-id="1248f-106">Cliente JavaScript</span><span class="sxs-lookup"><span data-stu-id="1248f-106">JavaScript client</span></span>

<span data-ttu-id="1248f-107">O [cliente JavaScript](https://www.npmjs.com/package/@aspnet/signalr) é executado no NodeJS 8 e versões posteriores e os seguintes navegadores:</span><span class="sxs-lookup"><span data-stu-id="1248f-107">The [JavaScript client](https://www.npmjs.com/package/@aspnet/signalr) runs on NodeJS 8 and later versions and the following browsers:</span></span>

| <span data-ttu-id="1248f-108">Navegador</span><span class="sxs-lookup"><span data-stu-id="1248f-108">Browser</span></span>                         | <span data-ttu-id="1248f-109">Versão</span><span class="sxs-lookup"><span data-stu-id="1248f-109">Version</span></span> |
| ------------------------------- | ------- |
| <span data-ttu-id="1248f-110">Microsoft Edge</span><span class="sxs-lookup"><span data-stu-id="1248f-110">Microsoft Edge</span></span>                  | <span data-ttu-id="1248f-111">atual</span><span class="sxs-lookup"><span data-stu-id="1248f-111">current</span></span> |
| <span data-ttu-id="1248f-112">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="1248f-112">Mozilla Firefox</span></span>                 | <span data-ttu-id="1248f-113">atual</span><span class="sxs-lookup"><span data-stu-id="1248f-113">current</span></span> |
| <span data-ttu-id="1248f-114">Google Chrome; inclui o Android</span><span class="sxs-lookup"><span data-stu-id="1248f-114">Google Chrome; includes Android</span></span> | <span data-ttu-id="1248f-115">atual</span><span class="sxs-lookup"><span data-stu-id="1248f-115">current</span></span> |
| <span data-ttu-id="1248f-116">Safari; inclui o iOS</span><span class="sxs-lookup"><span data-stu-id="1248f-116">Safari; includes iOS</span></span>            | <span data-ttu-id="1248f-117">atual</span><span class="sxs-lookup"><span data-stu-id="1248f-117">current</span></span> |
| <span data-ttu-id="1248f-118">Microsoft Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="1248f-118">Microsoft Internet Explorer</span></span>     | <span data-ttu-id="1248f-119">11</span><span class="sxs-lookup"><span data-stu-id="1248f-119">11</span></span>      |
 
## <a name="net-client"></a><span data-ttu-id="1248f-120">Cliente .NET</span><span class="sxs-lookup"><span data-stu-id="1248f-120">.NET client</span></span>

<span data-ttu-id="1248f-121">O [cliente .NET](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) é executado em qualquer plataforma com suporte pelo ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1248f-121">The [.NET client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) runs on any platform supported by ASP.NET Core.</span></span> <span data-ttu-id="1248f-122">Por exemplo, [desenvolvedores Xamarin podem usar o SignalR](https://github.com/aspnet/Announcements/issues/305) para a criação de aplicativos Android usando o xamarin. Android 8.4.0.1 e posterior e aplicativos iOS usando xamarin. IOS 11.14.0.4 e versões posteriores.</span><span class="sxs-lookup"><span data-stu-id="1248f-122">For example, [Xamarin developers can use SignalR](https://github.com/aspnet/Announcements/issues/305) for building Android apps using Xamarin.Android 8.4.0.1 and later and iOS apps using Xamarin.iOS 11.14.0.4 and later.</span></span>

<span data-ttu-id="1248f-123">Se o servidor executa o IIS, o transporte de WebSockets requer o IIS 8.0 ou superior no Windows Server 2012 ou superior.</span><span class="sxs-lookup"><span data-stu-id="1248f-123">If the server runs IIS, the WebSockets transport requires IIS 8.0 or higher on Windows Server 2012 or higher.</span></span> <span data-ttu-id="1248f-124">Outros transportes têm suporte em todas as plataformas.</span><span class="sxs-lookup"><span data-stu-id="1248f-124">Other transports are supported on all platforms.</span></span>

## <a name="java-client"></a><span data-ttu-id="1248f-125">Cliente de Java</span><span class="sxs-lookup"><span data-stu-id="1248f-125">Java client</span></span>

<span data-ttu-id="1248f-126">O [cliente Java](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) dá suporte a Java 8 e versões posteriores.</span><span class="sxs-lookup"><span data-stu-id="1248f-126">The [Java client](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) supports Java 8 and later versions.</span></span>

## <a name="unsupported-clients"></a><span data-ttu-id="1248f-127">Não há suporte para clientes</span><span class="sxs-lookup"><span data-stu-id="1248f-127">Unsupported clients</span></span>

<span data-ttu-id="1248f-128">Os clientes a seguir estão disponíveis, mas são experimentais ou não oficial.</span><span class="sxs-lookup"><span data-stu-id="1248f-128">The following clients are available but are experimental or unofficial.</span></span> <span data-ttu-id="1248f-129">Eles não têm suporte no momento e nunca podem ser.</span><span class="sxs-lookup"><span data-stu-id="1248f-129">They aren't currently supported and may never be.</span></span>

* [<span data-ttu-id="1248f-130">Cliente C++</span><span class="sxs-lookup"><span data-stu-id="1248f-130">C++ client</span></span>](https://github.com/aspnet/SignalR/tree/master/clients/cpp)

* [<span data-ttu-id="1248f-131">Cliente SWIFT</span><span class="sxs-lookup"><span data-stu-id="1248f-131">Swift client</span></span>](https://github.com/moozzyk/SignalR-Client-Swift)
