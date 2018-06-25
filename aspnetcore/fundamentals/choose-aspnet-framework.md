---
title: Escolher entre o ASP.NET e o ASP.NET Core
author: rick-anderson
description: Saiba como escolher entre o ASP.NET e o ASP.NET Core.
ms.author: riande
ms.date: 05/11/2018
uid: fundamentals/choose-between-aspnet-and-aspnetcore
ms.openlocfilehash: 6d759c0bc5e5c7d32d6c14786db6ba9fe7a2f1e8
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36297225"
---
# <a name="choose-between-aspnet-and-aspnet-core"></a><span data-ttu-id="0c2d7-103">Escolher entre o ASP.NET e o ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0c2d7-103">Choose between ASP.NET and ASP.NET Core</span></span>

<span data-ttu-id="0c2d7-104">Independentemente do aplicativo Web que você esteja criando, o ASP.NET tem uma solução para você: de aplicativos Web empresariais direcionados ao Windows Server a microsserviços pequenos direcionados a contêineres do Linux e tudo o que há entre essas duas categorias.</span><span class="sxs-lookup"><span data-stu-id="0c2d7-104">No matter the web app you're creating, ASP.NET has a solution for you: from enterprise web apps targeting Windows Server, to small microservices targeting Linux containers, and everything in between.</span></span>

## <a name="aspnet-core"></a><span data-ttu-id="0c2d7-105">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0c2d7-105">ASP.NET Core</span></span>

<span data-ttu-id="0c2d7-106">O ASP.NET Core é uma estrutura de software livre, multiplataforma, para a criação de aplicativos Web modernos e baseados em nuvem, no Windows, no macOS ou no Linux.</span><span class="sxs-lookup"><span data-stu-id="0c2d7-106">ASP.NET Core is an open-source, cross-platform framework for building modern, cloud-based web apps on Windows, macOS, or Linux.</span></span>

## <a name="aspnet"></a><span data-ttu-id="0c2d7-107">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="0c2d7-107">ASP.NET</span></span>

<span data-ttu-id="0c2d7-108">O ASP.NET é uma estrutura consolidada que fornece todos os serviços necessários para criar aplicativos Web baseados em servidor, de nível empresarial, no Windows.</span><span class="sxs-lookup"><span data-stu-id="0c2d7-108">ASP.NET is a mature framework that provides all the services needed to build enterprise-grade, server-based web apps on Windows.</span></span>

## <a name="framework-selection"></a><span data-ttu-id="0c2d7-109">Seleção de estrutura</span><span class="sxs-lookup"><span data-stu-id="0c2d7-109">Framework selection</span></span>

<span data-ttu-id="0c2d7-110">Examine a tabela abaixo para determinar qual estrutura é mais adequada às suas necessidades.</span><span class="sxs-lookup"><span data-stu-id="0c2d7-110">Review the table below to determine which framework is most appropriate for your needs.</span></span>

| <span data-ttu-id="0c2d7-111">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0c2d7-111">ASP.NET Core</span></span> | <span data-ttu-id="0c2d7-112">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="0c2d7-112">ASP.NET</span></span> |
|---|---|
|<span data-ttu-id="0c2d7-113">Build para Windows, macOS ou Linux</span><span class="sxs-lookup"><span data-stu-id="0c2d7-113">Build for Windows, macOS, or Linux</span></span>|<span data-ttu-id="0c2d7-114">Build para Windows</span><span class="sxs-lookup"><span data-stu-id="0c2d7-114">Build for Windows</span></span>|
|<span data-ttu-id="0c2d7-115">[Páginas Razor](xref:razor-pages/index) é a abordagem recomendada para criar uma interface do usuário da Web começando com o ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="0c2d7-115">[Razor Pages](xref:razor-pages/index) is the recommended approach to create a Web UI as of ASP.NET Core 2.x.</span></span> <span data-ttu-id="0c2d7-116">Confira também [MVC](xref:mvc/overview), [API Web](xref:tutorials/first-web-api) e [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="0c2d7-116">See also [MVC](xref:mvc/overview), [Web API](xref:tutorials/first-web-api), and [SignalR](xref:signalr/introduction).</span></span>|<span data-ttu-id="0c2d7-117">Use o [Web Forms](/aspnet/web-forms), o [SignalR](/aspnet/signalr), o [MVC](/aspnet/mvc), a [API Web](/aspnet/web-api/), [Webhooks](/aspnet/webhooks/) ou [páginas da Web](/aspnet/web-pages)</span><span class="sxs-lookup"><span data-stu-id="0c2d7-117">Use [Web Forms](/aspnet/web-forms), [SignalR](/aspnet/signalr), [MVC](/aspnet/mvc), [Web API](/aspnet/web-api/), [WebHooks](/aspnet/webhooks/), or [Web Pages](/aspnet/web-pages)</span></span>|
|<span data-ttu-id="0c2d7-118">Várias versões por computador</span><span class="sxs-lookup"><span data-stu-id="0c2d7-118">Multiple versions per machine</span></span>|<span data-ttu-id="0c2d7-119">Uma versão por computador</span><span class="sxs-lookup"><span data-stu-id="0c2d7-119">One version per machine</span></span>|
|<span data-ttu-id="0c2d7-120">Desenvolva com o Visual Studio, [Visual Studio para Mac](https://www.visualstudio.com/vs/visual-studio-mac/) ou [Visual Studio Code](https://code.visualstudio.com/) usando o C# ou o F#</span><span class="sxs-lookup"><span data-stu-id="0c2d7-120">Develop with Visual Studio, [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/), or [Visual Studio Code](https://code.visualstudio.com/) using C# or F#</span></span>|<span data-ttu-id="0c2d7-121">Desenvolva com o Visual Studio usando o C#, VB ou F#</span><span class="sxs-lookup"><span data-stu-id="0c2d7-121">Develop with Visual Studio using C#, VB, or F#</span></span>|
|<span data-ttu-id="0c2d7-122">Desempenho superior ao ASP.NET</span><span class="sxs-lookup"><span data-stu-id="0c2d7-122">Higher performance than ASP.NET</span></span>|<span data-ttu-id="0c2d7-123">Bom desempenho</span><span class="sxs-lookup"><span data-stu-id="0c2d7-123">Good performance</span></span>|
|[<span data-ttu-id="0c2d7-124">Escolha o .NET Framework ou o tempo de execução do .NET Core</span><span class="sxs-lookup"><span data-stu-id="0c2d7-124">Choose .NET Framework or .NET Core runtime</span></span>](/dotnet/articles/standard/choosing-core-framework-server)|<span data-ttu-id="0c2d7-125">Use o tempo de execução do .NET Framework</span><span class="sxs-lookup"><span data-stu-id="0c2d7-125">Use .NET Framework runtime</span></span>|

## <a name="aspnet-core-scenarios"></a><span data-ttu-id="0c2d7-126">Cenários do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0c2d7-126">ASP.NET Core scenarios</span></span>

* <span data-ttu-id="0c2d7-127">[Páginas Razor](xref:razor-pages/index) é a abordagem recomendada para criar uma interface do usuário da Web começando com o ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="0c2d7-127">[Razor Pages](xref:razor-pages/index) is the recommended approach to create a Web UI as of ASP.NET Core 2.x.</span></span>
* [<span data-ttu-id="0c2d7-128">Sites</span><span class="sxs-lookup"><span data-stu-id="0c2d7-128">Websites</span></span>](xref:tutorials/first-mvc-app/index)
* [<span data-ttu-id="0c2d7-129">APIs</span><span class="sxs-lookup"><span data-stu-id="0c2d7-129">APIs</span></span>](xref:tutorials/first-web-api)
* [<span data-ttu-id="0c2d7-130">Em tempo real</span><span class="sxs-lookup"><span data-stu-id="0c2d7-130">Real-time</span></span>](xref:signalr/index)

## <a name="aspnet-scenarios"></a><span data-ttu-id="0c2d7-131">Cenários do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="0c2d7-131">ASP.NET scenarios</span></span>

* [<span data-ttu-id="0c2d7-132">Sites</span><span class="sxs-lookup"><span data-stu-id="0c2d7-132">Websites</span></span>](/aspnet/mvc)
* [<span data-ttu-id="0c2d7-133">APIs</span><span class="sxs-lookup"><span data-stu-id="0c2d7-133">APIs</span></span>](/aspnet/web-api)
* [<span data-ttu-id="0c2d7-134">Em tempo real</span><span class="sxs-lookup"><span data-stu-id="0c2d7-134">Real-time</span></span>](/aspnet/signalr)

## <a name="resources"></a><span data-ttu-id="0c2d7-135">Recursos</span><span class="sxs-lookup"><span data-stu-id="0c2d7-135">Resources</span></span>

* [<span data-ttu-id="0c2d7-136">Introdução ao ASP.NET</span><span class="sxs-lookup"><span data-stu-id="0c2d7-136">Introduction to ASP.NET</span></span>](/aspnet/overview)
* [<span data-ttu-id="0c2d7-137">Introdução ao ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0c2d7-137">Introduction to ASP.NET Core</span></span>](xref:index)
