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
ms.openlocfilehash: 7f8ccff7e3da93d6e617505ac93fafc3a82ed880
ms.sourcegitcommit: 63fb07fb3f71b32daf2c9466e132f2e7cc617163
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/10/2018
ms.locfileid: "35252003"
---
# <a name="host-in-aspnet-core"></a><span data-ttu-id="24b43-103">Host no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="24b43-103">Host in ASP.NET Core</span></span>

<span data-ttu-id="24b43-104">Aplicativos ASP.NET Core configuram e inicializam um *host*.</span><span class="sxs-lookup"><span data-stu-id="24b43-104">.NET apps configure and launch a *host*.</span></span> <span data-ttu-id="24b43-105">O host é responsável pelo gerenciamento de tempo de vida e pela inicialização do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="24b43-105">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="24b43-106">Duas APIs de host estão disponíveis para uso:</span><span class="sxs-lookup"><span data-stu-id="24b43-106">Two host APIs are available for use:</span></span>

* <span data-ttu-id="24b43-107">[Host da Web](xref:fundamentals/host/web-host) &ndash; Adequado para a hospedagem de aplicativos Web.</span><span class="sxs-lookup"><span data-stu-id="24b43-107">[Web Host](xref:fundamentals/host/web-host) &ndash; Suitable for hosting web apps.</span></span>
* <span data-ttu-id="24b43-108">[Host Genérico](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 ou posteriores) &ndash; Adequado para a hospedagem de aplicativos não Web (por exemplo, aplicativos que executam tarefas em segundo plano).</span><span class="sxs-lookup"><span data-stu-id="24b43-108">[Generic Host](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 or later) &ndash; Suitable for hosting non-web apps (for example, apps that run background tasks).</span></span> <span data-ttu-id="24b43-109">Em uma versão futura, o Host Genérico será adequado para hospedar qualquer tipo de aplicativo, incluindo aplicativos Web.</span><span class="sxs-lookup"><span data-stu-id="24b43-109">In a future release, the Generic Host will be suitable for hosting any kind of app, including web apps.</span></span> <span data-ttu-id="24b43-110">Eventualmente, o Host Genérico substituirá o Host da Web.</span><span class="sxs-lookup"><span data-stu-id="24b43-110">The Generic Host will eventually replace the Web Host.</span></span>

<span data-ttu-id="24b43-111">Para hospedar *aplicativos Web* do ASP.NET Core, os desenvolvedores devem usar o Web Host com base em [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="24b43-111">For hosting ASP.NET Core *web apps*, developers should use the Web Host based on [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="24b43-112">Para hospedar *aplicativos que não sejam Web*, os desenvolvedores devem usar o Host Genérico com base em [HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder).</span><span class="sxs-lookup"><span data-stu-id="24b43-112">For hosting *non-web apps*, developers should use the Generic Host based on [HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder).</span></span>
