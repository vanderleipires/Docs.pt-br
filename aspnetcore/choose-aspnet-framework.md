---
title: Escolher entre o ASP.NET e o ASP.NET Core
author: rick-anderson
description: Saiba como escolher entre o ASP.NET e o ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 03/14/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/choose-between-aspnet-and-aspnetcore
ms.openlocfilehash: e6ac9f54ef623895b81eea33d90791e5f0ad5120
ms.sourcegitcommit: 71b93b42cbce8a9b1a12c4d88391e75a4dfb6162
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/20/2018
---
# <a name="choose-between-aspnet-and-aspnet-core"></a><span data-ttu-id="06a13-103">Escolher entre o ASP.NET e o ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="06a13-103">Choose between ASP.NET and ASP.NET Core</span></span>

<span data-ttu-id="06a13-104">Independentemente do aplicativo Web que você esteja criando, o ASP.NET tem uma solução para você: de aplicativos Web empresariais direcionados ao Windows Server a microsserviços pequenos direcionados a contêineres do Linux e tudo o que há entre essas duas categorias.</span><span class="sxs-lookup"><span data-stu-id="06a13-104">No matter the web app you're creating, ASP.NET has a solution for you: from enterprise web apps targeting Windows Server, to small microservices targeting Linux containers, and everything in between.</span></span>

## <a name="aspnet-core"></a><span data-ttu-id="06a13-105">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="06a13-105">ASP.NET Core</span></span>

<span data-ttu-id="06a13-106">O ASP.NET Core é uma estrutura de software livre, multiplataforma, para a criação de aplicativos Web modernos e baseados em nuvem, no Windows, no macOS ou no Linux.</span><span class="sxs-lookup"><span data-stu-id="06a13-106">ASP.NET Core is an open-source, cross-platform framework for building modern, cloud-based web apps on Windows, macOS, or Linux.</span></span>

## <a name="aspnet"></a><span data-ttu-id="06a13-107">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="06a13-107">ASP.NET</span></span>

<span data-ttu-id="06a13-108">O ASP.NET é uma estrutura consolidada que fornece todos os serviços necessários para criar aplicativos Web baseados em servidor, de nível empresarial, no Windows.</span><span class="sxs-lookup"><span data-stu-id="06a13-108">ASP.NET is a mature framework that provides all the services needed to build enterprise-class, server-based web apps on Windows.</span></span>

## <a name="which-one-is-right-for-me"></a><span data-ttu-id="06a13-109">Qual é a opção ideal para mim?</span><span class="sxs-lookup"><span data-stu-id="06a13-109">Which one is right for me?</span></span>

| <span data-ttu-id="06a13-110">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="06a13-110">ASP.NET Core</span></span> | <span data-ttu-id="06a13-111">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="06a13-111">ASP.NET</span></span> |
|---|---|
|<span data-ttu-id="06a13-112">Build para Windows, macOS ou Linux</span><span class="sxs-lookup"><span data-stu-id="06a13-112">Build for Windows, macOS, or Linux</span></span>|<span data-ttu-id="06a13-113">Build para Windows</span><span class="sxs-lookup"><span data-stu-id="06a13-113">Build for Windows</span></span>|
|<span data-ttu-id="06a13-114">[Páginas Razor](xref:mvc/razor-pages/index) é a abordagem recomendada para criar uma interface do usuário da Web começando com o ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="06a13-114">[Razor Pages](xref:mvc/razor-pages/index) is the recommended approach to create a Web UI as of ASP.NET Core 2.x.</span></span> <span data-ttu-id="06a13-115">Confira também [MVC](xref:mvc/overview), [API Web](xref:tutorials/first-web-api) e [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="06a13-115">See also [MVC](xref:mvc/overview), [Web API](xref:tutorials/first-web-api), and [SignalR](xref:signalr/introduction).</span></span>|<span data-ttu-id="06a13-116">Use o [Web Forms](/aspnet/web-forms), o [SignalR](/aspnet/signalr), o [MVC](/aspnet/mvc), a [API Web](/aspnet/web-api/) ou [páginas da Web](/aspnet/web-pages)</span><span class="sxs-lookup"><span data-stu-id="06a13-116">Use [Web Forms](/aspnet/web-forms), [SignalR](/aspnet/signalr), [MVC](/aspnet/mvc), [Web API](/aspnet/web-api/), or [Web Pages](/aspnet/web-pages)</span></span>|
|<span data-ttu-id="06a13-117">Várias versões por computador</span><span class="sxs-lookup"><span data-stu-id="06a13-117">Multiple versions per machine</span></span>|<span data-ttu-id="06a13-118">Uma versão por computador</span><span class="sxs-lookup"><span data-stu-id="06a13-118">One version per machine</span></span>|
|<span data-ttu-id="06a13-119">Desenvolva com o Visual Studio, [Visual Studio para Mac](https://www.visualstudio.com/vs/visual-studio-mac/) ou [Visual Studio Code](https://code.visualstudio.com/) usando o C# ou o F#</span><span class="sxs-lookup"><span data-stu-id="06a13-119">Develop with Visual Studio, [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/), or [Visual Studio Code](https://code.visualstudio.com/) using C# or F#</span></span>|<span data-ttu-id="06a13-120">Desenvolva com o Visual Studio usando o C#, VB ou F#</span><span class="sxs-lookup"><span data-stu-id="06a13-120">Develop with Visual Studio using C#, VB, or F#</span></span>|
|<span data-ttu-id="06a13-121">Desempenho superior ao ASP.NET</span><span class="sxs-lookup"><span data-stu-id="06a13-121">Higher performance than ASP.NET</span></span>|<span data-ttu-id="06a13-122">Bom desempenho</span><span class="sxs-lookup"><span data-stu-id="06a13-122">Good performance</span></span>|
|[<span data-ttu-id="06a13-123">Escolha o .NET Framework ou o tempo de execução do .NET Core</span><span class="sxs-lookup"><span data-stu-id="06a13-123">Choose .NET Framework or .NET Core runtime</span></span>](/dotnet/articles/standard/choosing-core-framework-server)|<span data-ttu-id="06a13-124">Use o tempo de execução do .NET Framework</span><span class="sxs-lookup"><span data-stu-id="06a13-124">Use .NET Framework runtime</span></span>|

## <a name="aspnet-core-scenarios"></a><span data-ttu-id="06a13-125">Cenários do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="06a13-125">ASP.NET Core scenarios</span></span>

<!-- update link to Razor Pages mvc movie series when done -->
* <span data-ttu-id="06a13-126">[Páginas Razor](xref:mvc/razor-pages/index) é a abordagem recomendada para criar uma interface do usuário da Web começando com o ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="06a13-126">[Razor Pages](xref:mvc/razor-pages/index) is the recommended approach to create a Web UI as of ASP.NET Core 2.x.</span></span>
* [<span data-ttu-id="06a13-127">Sites</span><span class="sxs-lookup"><span data-stu-id="06a13-127">Websites</span></span>](xref:tutorials/first-mvc-app/index)
* [<span data-ttu-id="06a13-128">APIs</span><span class="sxs-lookup"><span data-stu-id="06a13-128">APIs</span></span>](xref:tutorials/first-web-api)
* [<span data-ttu-id="06a13-129">Em tempo real</span><span class="sxs-lookup"><span data-stu-id="06a13-129">Real-time</span></span>](xref:signalr/index)

## <a name="aspnet-scenarios"></a><span data-ttu-id="06a13-130">Cenários do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="06a13-130">ASP.NET scenarios</span></span>

* [<span data-ttu-id="06a13-131">Sites</span><span class="sxs-lookup"><span data-stu-id="06a13-131">Websites</span></span>](/aspnet/mvc)
* [<span data-ttu-id="06a13-132">APIs</span><span class="sxs-lookup"><span data-stu-id="06a13-132">APIs</span></span>](/aspnet/web-api)
* [<span data-ttu-id="06a13-133">Em tempo real</span><span class="sxs-lookup"><span data-stu-id="06a13-133">Real-time</span></span>](/aspnet/signalr)

## <a name="resources"></a><span data-ttu-id="06a13-134">Recursos</span><span class="sxs-lookup"><span data-stu-id="06a13-134">Resources</span></span>

* [<span data-ttu-id="06a13-135">Introdução ao ASP.NET</span><span class="sxs-lookup"><span data-stu-id="06a13-135">Introduction to ASP.NET</span></span>](/aspnet/overview)
* [<span data-ttu-id="06a13-136">Introdução ao ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="06a13-136">Introduction to ASP.NET Core</span></span>](xref:index)
