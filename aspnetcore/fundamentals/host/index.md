---
title: Host no ASP.NET Core
author: guardrex
description: Saiba mais sobre o Host da Web do ASP.NET Core e o Host Genérico do .NET, que são responsáveis pelo gerenciamento de tempo de vida e pela inicialização do aplicativo.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/host/index
ms.openlocfilehash: 7ad059e39866f59040c12b7ac15e9fa3405a9aad
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/17/2018
---
# <a name="host-in-aspnet-core"></a><span data-ttu-id="7a407-103">Host no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7a407-103">Host in ASP.NET Core</span></span>

<span data-ttu-id="7a407-104">Aplicativos ASP.NET Core configuram e inicializam um *host*.</span><span class="sxs-lookup"><span data-stu-id="7a407-104">.NET apps configure and launch a *host*.</span></span> <span data-ttu-id="7a407-105">O host é responsável pelo gerenciamento de tempo de vida e pela inicialização do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="7a407-105">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="7a407-106">Duas APIs de host estão disponíveis para uso:</span><span class="sxs-lookup"><span data-stu-id="7a407-106">Two host APIs are available for use:</span></span>

* <span data-ttu-id="7a407-107">[Host da Web](xref:fundamentals/host/web-host) &ndash; Adequado para a hospedagem de aplicativos Web.</span><span class="sxs-lookup"><span data-stu-id="7a407-107">[Web Host](xref:fundamentals/host/web-host) &ndash; Suitable for hosting web apps.</span></span>
* <span data-ttu-id="7a407-108">[Host Genérico](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 ou posteriores) &ndash; Adequado para a hospedagem de aplicativos não Web (por exemplo, aplicativos que executam tarefas em segundo plano).</span><span class="sxs-lookup"><span data-stu-id="7a407-108">[Generic Host](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 or later) &ndash; Suitable for hosting non-web apps (for example, apps that run background tasks).</span></span> <span data-ttu-id="7a407-109">Em uma versão futura, o Host Genérico será adequado para hospedar qualquer tipo de aplicativo, incluindo aplicativos Web.</span><span class="sxs-lookup"><span data-stu-id="7a407-109">In a future release, the Generic Host will be suitable for hosting any kind of app, including web apps.</span></span> <span data-ttu-id="7a407-110">Eventualmente, o Host Genérico substituirá o Host da Web.</span><span class="sxs-lookup"><span data-stu-id="7a407-110">The Generic Host will eventually replace the Web Host.</span></span>

<span data-ttu-id="7a407-111">Neste momento, os desenvolvedores devem usar o [Host da Web](xref:fundamentals/host/web-host) com base no [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) para hospedar aplicativos ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7a407-111">At this time, developers should use the [Web Host](xref:fundamentals/host/web-host) based on [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) for hosting ASP.NET Core apps.</span></span>
