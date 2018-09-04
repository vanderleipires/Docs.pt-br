---
title: Host no ASP.NET Core
author: guardrex
description: Saiba mais sobre o Host da Web do ASP.NET Core e o Host Genérico do .NET, que são responsáveis pelo gerenciamento de tempo de vida e pela inicialização do aplicativo.
ms.author: riande
ms.custom: mvc
ms.date: 08/28/2018
uid: fundamentals/host/index
ms.openlocfilehash: 9927722b5080beb94e5628d9e7b54e6d50a5bff8
ms.sourcegitcommit: a669c4e3f42e387e214a354ac4143555602e6f66
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/31/2018
ms.locfileid: "43336044"
---
# <a name="host-in-aspnet-core"></a><span data-ttu-id="4c176-103">Host no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4c176-103">Host in ASP.NET Core</span></span>

<span data-ttu-id="4c176-104">Aplicativos ASP.NET Core configuram e inicializam um *host*.</span><span class="sxs-lookup"><span data-stu-id="4c176-104">.NET apps configure and launch a *host*.</span></span> <span data-ttu-id="4c176-105">O host é responsável pelo gerenciamento de tempo de vida e pela inicialização do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="4c176-105">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="4c176-106">Duas APIs de host estão disponíveis para uso:</span><span class="sxs-lookup"><span data-stu-id="4c176-106">Two host APIs are available for use:</span></span>

* <span data-ttu-id="4c176-107">[Host da Web](xref:fundamentals/host/web-host) &ndash; Adequado para a hospedagem de aplicativos Web.</span><span class="sxs-lookup"><span data-stu-id="4c176-107">[Web Host](xref:fundamentals/host/web-host) &ndash; Suitable for hosting web apps.</span></span>
* <span data-ttu-id="4c176-108">[Host Genérico](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 ou posteriores) &ndash; Adequado para a hospedagem de aplicativos não Web (por exemplo, aplicativos que executam tarefas em segundo plano).</span><span class="sxs-lookup"><span data-stu-id="4c176-108">[Generic Host](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 or later) &ndash; Suitable for hosting non-web apps (for example, apps that run background tasks).</span></span> <span data-ttu-id="4c176-109">Em uma versão futura, o Host Genérico será adequado para hospedar qualquer tipo de aplicativo, incluindo aplicativos Web.</span><span class="sxs-lookup"><span data-stu-id="4c176-109">In a future release, the Generic Host will be suitable for hosting any kind of app, including web apps.</span></span> <span data-ttu-id="4c176-110">Eventualmente, o Host Genérico substituirá o Host da Web.</span><span class="sxs-lookup"><span data-stu-id="4c176-110">The Generic Host will eventually replace the Web Host.</span></span>

<span data-ttu-id="4c176-111">Para hospedar *aplicativos Web* do ASP.NET Core, os desenvolvedores devem usar o Web Host com base em <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>.</span><span class="sxs-lookup"><span data-stu-id="4c176-111">For hosting ASP.NET Core *web apps*, developers should use the Web Host based on <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>.</span></span> <span data-ttu-id="4c176-112">Para hospedar *aplicativos que não sejam Web*, os desenvolvedores devem usar o Host Genérico com base em <xref:Microsoft.Extensions.Hosting.HostBuilder>.</span><span class="sxs-lookup"><span data-stu-id="4c176-112">For hosting *non-web apps*, developers should use the Generic Host based on <xref:Microsoft.Extensions.Hosting.HostBuilder>.</span></span>

<xref:fundamentals/host/hosted-services>  
<span data-ttu-id="4c176-113">Aprenda a implementar tarefas em segundo plano com serviços hospedados no ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4c176-113">Learn how to implement background tasks with hosted services in ASP.NET Core.</span></span>

<xref:fundamentals/configuration/platform-specific-configuration>  
<span data-ttu-id="4c176-114">Descubra como aprimorar um aplicativo ASP.NET Core por meio de um assembly referenciado ou não referenciado usando uma implementação de <xref:Microsoft.AspNetCore.Hosting.IHostingStartup>.</span><span class="sxs-lookup"><span data-stu-id="4c176-114">Discover how to enhance an ASP.NET Core app from a referenced or unreferenced assembly using an <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation.</span></span>
