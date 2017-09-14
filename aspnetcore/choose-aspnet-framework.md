---
title: Escolher entre o ASP.NET e o ASP.NET Core
author: rick-anderson
description: Como escolher entre o ASP.NET e o ASP.NET Core.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2017
ms.topic: article
ms.assetid: f0d17abf-3c69-413e-87fc-30780805e33f
ms.technology: aspnet
ms.prod: asp.net-core
msc.legacyurl: /learn
msc.type: content
ms.openlocfilehash: b9c8a54ceea3ff1c1c9bcff692c884761b17fd73
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2017
---
# <a name="choose-between-aspnet-and-aspnet-core"></a><span data-ttu-id="0c497-103">Escolher entre o ASP.NET e o ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0c497-103">Choose between ASP.NET and ASP.NET Core</span></span> 

<span data-ttu-id="0c497-104">Independentemente do aplicativo Web que você está criando, o ASP.NET tem uma solução para você: de aplicativos Web empresariais direcionados ao Windows Server a microsserviços pequenos direcionados a contêineres do Linux e tudo o que há entre essas duas categorias.</span><span class="sxs-lookup"><span data-stu-id="0c497-104">No matter the web application you are creating, ASP.NET has a solution for you: from enterprise web applications targeting Windows Server, to small microservices targeting Linux containers, and everything in between.</span></span>

## <a name="aspnet-core"></a><span data-ttu-id="0c497-105">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0c497-105">ASP.NET Core</span></span>

<span data-ttu-id="0c497-106">O ASP.NET Core é uma estrutura de software livre e de multiplaforma para a criação de aplicativos Web modernos e baseados em nuvem no Windows, macOS ou Linux.</span><span class="sxs-lookup"><span data-stu-id="0c497-106">ASP.NET Core is an open-source, cross-platform framework for building modern, cloud-based web applications on Windows, macOS, or Linux.</span></span>

## <a name="aspnet"></a><span data-ttu-id="0c497-107">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="0c497-107">ASP.NET</span></span>

<span data-ttu-id="0c497-108">O ASP.NET é uma estrutura consolidada que fornece todos os serviços necessários para criar aplicativos Web baseados em servidor de nível empresarial no Windows.</span><span class="sxs-lookup"><span data-stu-id="0c497-108">ASP.NET is a mature framework that provides all the services needed to build enterprise-class, server-based web applications on Windows.</span></span>

## <a name="which-one-is-right-for-me"></a><span data-ttu-id="0c497-109">Qual é a opção ideal para mim?</span><span class="sxs-lookup"><span data-stu-id="0c497-109">Which one is right for me?</span></span>

| <span data-ttu-id="0c497-110">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0c497-110">ASP.NET Core</span></span> | <span data-ttu-id="0c497-111">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="0c497-111">ASP.NET</span></span> |
|---|---|
|<span data-ttu-id="0c497-112">Build para Windows, macOS ou Linux</span><span class="sxs-lookup"><span data-stu-id="0c497-112">Build for Windows, macOS, or Linux</span></span>|<span data-ttu-id="0c497-113">Build para Windows</span><span class="sxs-lookup"><span data-stu-id="0c497-113">Build for Windows</span></span>|
|<span data-ttu-id="0c497-114">As [Páginas Razor](xref:mvc/razor-pages/index) são a abordagem recomendada para criar uma interface do usuário da Web com o ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="0c497-114">[Razor Pages](xref:mvc/razor-pages/index) is the recommended approach to create a Web UI with ASP.NET Core 2.0.</span></span> <span data-ttu-id="0c497-115">Consulte também [MVC](xref:mvc/overview) e [API Web](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="0c497-115">See also [MVC](xref:mvc/overview) and [Web API](xref:tutorials/first-web-api)</span></span>|<span data-ttu-id="0c497-116">Use o [Web Forms](https://docs.microsoft.com/aspnet/web-forms), o [SignalR](https://docs.microsoft.com/aspnet/signalr), o [MVC](https://docs.microsoft.com/aspnet/mvc), a [API Web](https://docs.microsoft.com/aspnet/web-api/) ou [páginas da Web](https://docs.microsoft.com/aspnet/web-pages)</span><span class="sxs-lookup"><span data-stu-id="0c497-116">Use [Web Forms](https://docs.microsoft.com/aspnet/web-forms), [SignalR](https://docs.microsoft.com/aspnet/signalr), [MVC](https://docs.microsoft.com/aspnet/mvc), [Web API](https://docs.microsoft.com/aspnet/web-api/), or [Web Pages](https://docs.microsoft.com/aspnet/web-pages)</span></span>|
|<span data-ttu-id="0c497-117">Várias versões por computador</span><span class="sxs-lookup"><span data-stu-id="0c497-117">Multiple versions per machine</span></span>|<span data-ttu-id="0c497-118">Uma versão por computador</span><span class="sxs-lookup"><span data-stu-id="0c497-118">One version per machine</span></span>|
|<span data-ttu-id="0c497-119">Desenvolva com o Visual Studio, [Visual Studio para Mac](https://www.visualstudio.com/vs/visual-studio-mac/) ou [Visual Studio Code](https://code.visualstudio.com/) usando o C# ou o F#</span><span class="sxs-lookup"><span data-stu-id="0c497-119">Develop with Visual Studio, [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/), or [Visual Studio Code](https://code.visualstudio.com/) using C# or F#</span></span>|<span data-ttu-id="0c497-120">Desenvolva com o Visual Studio usando o C#, VB ou F#</span><span class="sxs-lookup"><span data-stu-id="0c497-120">Develop with Visual Studio using C#, VB, or F#</span></span>|
|<span data-ttu-id="0c497-121">Desempenho superior ao ASP.NET</span><span class="sxs-lookup"><span data-stu-id="0c497-121">Higher performance than ASP.NET</span></span>|<span data-ttu-id="0c497-122">Bom desempenho</span><span class="sxs-lookup"><span data-stu-id="0c497-122">Good performance</span></span>|
|[<span data-ttu-id="0c497-123">Escolha o .NET Framework ou o tempo de execução do .NET Core</span><span class="sxs-lookup"><span data-stu-id="0c497-123">Choose .NET Framework or .NET Core runtime</span></span>](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server)|<span data-ttu-id="0c497-124">Use o tempo de execução do .NET Framework</span><span class="sxs-lookup"><span data-stu-id="0c497-124">Use .NET Framework runtime</span></span>|

## <a name="aspnet-core-scenarios"></a><span data-ttu-id="0c497-125">Cenários do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0c497-125">ASP.NET Core scenarios</span></span>

<!-- update link to Razor Pages mvc movie series when done -->
* <span data-ttu-id="0c497-126">As [Páginas Razor](xref:mvc/razor-pages/index) são a abordagem recomendada para criar uma interface do usuário da Web com o ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="0c497-126">[Razor Pages](xref:mvc/razor-pages/index) is the recommended approach to create a Web UI with ASP.NET Core 2.0.</span></span>
* [<span data-ttu-id="0c497-127">Sites</span><span class="sxs-lookup"><span data-stu-id="0c497-127">Websites</span></span>](xref:tutorials/first-mvc-app/index)
* [<span data-ttu-id="0c497-128">APIs</span><span class="sxs-lookup"><span data-stu-id="0c497-128">APIs</span></span>](xref:tutorials/first-web-api)

## <a name="aspnet-scenarios"></a><span data-ttu-id="0c497-129">Cenários do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="0c497-129">ASP.NET scenarios</span></span>

* [<span data-ttu-id="0c497-130">Sites</span><span class="sxs-lookup"><span data-stu-id="0c497-130">Websites</span></span>](https://docs.microsoft.com/aspnet/mvc)
* [<span data-ttu-id="0c497-131">APIs</span><span class="sxs-lookup"><span data-stu-id="0c497-131">APIs</span></span>](https://docs.microsoft.com/aspnet/web-api)
* [<span data-ttu-id="0c497-132">Em tempo real</span><span class="sxs-lookup"><span data-stu-id="0c497-132">Real-time</span></span>](https://docs.microsoft.com/aspnet/signalr)

## <a name="resources"></a><span data-ttu-id="0c497-133">Recursos</span><span class="sxs-lookup"><span data-stu-id="0c497-133">Resources</span></span>

* [<span data-ttu-id="0c497-134">Introdução ao ASP.NET</span><span class="sxs-lookup"><span data-stu-id="0c497-134">Introduction to ASP.NET</span></span>](https://docs.microsoft.com/aspnet/overview)
* [<span data-ttu-id="0c497-135">Introdução ao ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0c497-135">Introduction to ASP.NET Core</span></span>](xref:index)
