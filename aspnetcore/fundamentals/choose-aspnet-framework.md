---
title: Escolher entre o ASP.NET 4.x e o ASP.NET Core
author: rick-anderson
description: Explica o ASP.NET Core vs. ASP.NET 4.x e como escolher entre eles.
ms.author: riande
ms.custom: seodec18
ms.date: 09/11/2018
uid: fundamentals/choose-between-aspnet-and-aspnetcore
ms.openlocfilehash: b75fbea330e48075c4a2789454e973d4c56ffa53
ms.sourcegitcommit: 49faca2644590fc081d86db46ea5e29edfc28b7b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/09/2018
ms.locfileid: "53121174"
---
# <a name="choose-between-aspnet-4x-and-aspnet-core"></a><span data-ttu-id="e80fc-103">Escolher entre o ASP.NET 4.x e o ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e80fc-103">Choose between ASP.NET 4.x and ASP.NET Core</span></span>

<span data-ttu-id="e80fc-104">O ASP.NET Core é uma reformulação do ASP.NET 4. x.</span><span class="sxs-lookup"><span data-stu-id="e80fc-104">ASP.NET Core is a redesign of ASP.NET 4.x.</span></span> <span data-ttu-id="e80fc-105">Este artigo lista as diferenças entre eles.</span><span class="sxs-lookup"><span data-stu-id="e80fc-105">This article lists the differences between them.</span></span>

## <a name="aspnet-core"></a><span data-ttu-id="e80fc-106">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e80fc-106">ASP.NET Core</span></span>

<span data-ttu-id="e80fc-107">O ASP.NET Core é uma estrutura de software livre, multiplataforma, para a criação de aplicativos Web modernos e baseados em nuvem, no Windows, no macOS ou no Linux.</span><span class="sxs-lookup"><span data-stu-id="e80fc-107">ASP.NET Core is an open-source, cross-platform framework for building modern, cloud-based web apps on Windows, macOS, or Linux.</span></span>

[!INCLUDE[](~/includes/benefits.md)]

## <a name="aspnet-4x"></a><span data-ttu-id="e80fc-108">ASP.NET 4.x</span><span class="sxs-lookup"><span data-stu-id="e80fc-108">ASP.NET 4.x</span></span>

<span data-ttu-id="e80fc-109">O ASP.NET 4.x é uma estrutura consolidada que fornece os serviços necessários para criar aplicativos Web baseados em servidor, de nível empresarial, no Windows.</span><span class="sxs-lookup"><span data-stu-id="e80fc-109">ASP.NET 4.x is a mature framework that provides the services needed to build enterprise-grade, server-based web apps on Windows.</span></span>

## <a name="framework-selection"></a><span data-ttu-id="e80fc-110">Seleção de estrutura</span><span class="sxs-lookup"><span data-stu-id="e80fc-110">Framework selection</span></span>

<span data-ttu-id="e80fc-111">A tabela a seguir compara o ASP.NET Core com o ASP.NET 4. x.</span><span class="sxs-lookup"><span data-stu-id="e80fc-111">The following table compares ASP.NET Core to ASP.NET 4.x.</span></span>

| <span data-ttu-id="e80fc-112">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e80fc-112">ASP.NET Core</span></span> | <span data-ttu-id="e80fc-113">ASP.NET 4.x</span><span class="sxs-lookup"><span data-stu-id="e80fc-113">ASP.NET 4.x</span></span> |
|---|---|
|<span data-ttu-id="e80fc-114">Build para Windows, macOS ou Linux</span><span class="sxs-lookup"><span data-stu-id="e80fc-114">Build for Windows, macOS, or Linux</span></span>|<span data-ttu-id="e80fc-115">Build para Windows</span><span class="sxs-lookup"><span data-stu-id="e80fc-115">Build for Windows</span></span>|
|<span data-ttu-id="e80fc-116">[Páginas Razor](xref:razor-pages/index) é a abordagem recomendada para criar uma interface do usuário da Web começando com o ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="e80fc-116">[Razor Pages](xref:razor-pages/index) is the recommended approach to create a Web UI as of ASP.NET Core 2.x.</span></span> <span data-ttu-id="e80fc-117">Confira também [MVC](xref:mvc/overview), [API Web](xref:tutorials/first-web-api) e [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="e80fc-117">See also [MVC](xref:mvc/overview), [Web API](xref:tutorials/first-web-api), and [SignalR](xref:signalr/introduction).</span></span>|<span data-ttu-id="e80fc-118">Use o [Web Forms](/aspnet/web-forms), o [SignalR](/aspnet/signalr), o [MVC](/aspnet/mvc), a [API Web](/aspnet/web-api/), [Webhooks](/aspnet/webhooks/) ou [páginas da Web](/aspnet/web-pages)</span><span class="sxs-lookup"><span data-stu-id="e80fc-118">Use [Web Forms](/aspnet/web-forms), [SignalR](/aspnet/signalr), [MVC](/aspnet/mvc), [Web API](/aspnet/web-api/), [WebHooks](/aspnet/webhooks/), or [Web Pages](/aspnet/web-pages)</span></span>|
|<span data-ttu-id="e80fc-119">Várias versões por computador</span><span class="sxs-lookup"><span data-stu-id="e80fc-119">Multiple versions per machine</span></span>|<span data-ttu-id="e80fc-120">Uma versão por computador</span><span class="sxs-lookup"><span data-stu-id="e80fc-120">One version per machine</span></span>|
|<span data-ttu-id="e80fc-121">Desenvolva com o Visual Studio, [Visual Studio para Mac](https://www.visualstudio.com/vs/visual-studio-mac/) ou [Visual Studio Code](https://code.visualstudio.com/) usando o C# ou o F#</span><span class="sxs-lookup"><span data-stu-id="e80fc-121">Develop with Visual Studio, [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/), or [Visual Studio Code](https://code.visualstudio.com/) using C# or F#</span></span>|<span data-ttu-id="e80fc-122">Desenvolva com o Visual Studio usando o C#, VB ou F#</span><span class="sxs-lookup"><span data-stu-id="e80fc-122">Develop with Visual Studio using C#, VB, or F#</span></span>|
|<span data-ttu-id="e80fc-123">Desempenho superior ao do ASP.NET 4.x</span><span class="sxs-lookup"><span data-stu-id="e80fc-123">Higher performance than ASP.NET 4.x</span></span>|<span data-ttu-id="e80fc-124">Bom desempenho</span><span class="sxs-lookup"><span data-stu-id="e80fc-124">Good performance</span></span>|
|[<span data-ttu-id="e80fc-125">Escolha o .NET Framework ou o tempo de execução do .NET Core</span><span class="sxs-lookup"><span data-stu-id="e80fc-125">Choose .NET Framework or .NET Core runtime</span></span>](/dotnet/standard/choosing-core-framework-server)|<span data-ttu-id="e80fc-126">Use o tempo de execução do .NET Framework</span><span class="sxs-lookup"><span data-stu-id="e80fc-126">Use .NET Framework runtime</span></span>|

<span data-ttu-id="e80fc-127">Confira [ASP.NET Core targeting .NET Framework](xref:index#target-framework) (ASP.NET Core direcionado para o .NET Framework) para obter informações sobre o suporte do ASP.NET Core 2.x no .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="e80fc-127">See [ASP.NET Core targeting .NET Framework](xref:index#target-framework) for information on ASP.NET Core 2.x support on .NET Framework.</span></span>

## <a name="aspnet-core-scenarios"></a><span data-ttu-id="e80fc-128">Cenários do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e80fc-128">ASP.NET Core scenarios</span></span>

* <span data-ttu-id="e80fc-129">[Páginas Razor](xref:razor-pages/index) é a abordagem recomendada para criar uma interface do usuário da Web começando com o ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="e80fc-129">[Razor Pages](xref:razor-pages/index) is the recommended approach to create a Web UI as of ASP.NET Core 2.x.</span></span>
* [<span data-ttu-id="e80fc-130">Sites</span><span class="sxs-lookup"><span data-stu-id="e80fc-130">Websites</span></span>](xref:tutorials/first-mvc-app/index)
* [<span data-ttu-id="e80fc-131">APIs</span><span class="sxs-lookup"><span data-stu-id="e80fc-131">APIs</span></span>](xref:tutorials/first-web-api)
* [<span data-ttu-id="e80fc-132">Em tempo real</span><span class="sxs-lookup"><span data-stu-id="e80fc-132">Real-time</span></span>](xref:signalr/index)
* [<span data-ttu-id="e80fc-133">Implantar um aplicativo ASP.NET Core no Azure</span><span class="sxs-lookup"><span data-stu-id="e80fc-133">Deploy an ASP.NET Core app to Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet)

## <a name="aspnet-4x-scenarios"></a><span data-ttu-id="e80fc-134">Cenários do ASP.NET 4.x</span><span class="sxs-lookup"><span data-stu-id="e80fc-134">ASP.NET 4.x scenarios</span></span>

* [<span data-ttu-id="e80fc-135">Sites</span><span class="sxs-lookup"><span data-stu-id="e80fc-135">Websites</span></span>](/aspnet/mvc)
* [<span data-ttu-id="e80fc-136">APIs</span><span class="sxs-lookup"><span data-stu-id="e80fc-136">APIs</span></span>](/aspnet/web-api)
* [<span data-ttu-id="e80fc-137">Em tempo real</span><span class="sxs-lookup"><span data-stu-id="e80fc-137">Real-time</span></span>](/aspnet/signalr)
* [<span data-ttu-id="e80fc-138">Criar um aplicativo Web ASP.NET 4.x no Azure</span><span class="sxs-lookup"><span data-stu-id="e80fc-138">Create an ASP.NET 4.x web app in Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet-framework)

## <a name="additional-resources"></a><span data-ttu-id="e80fc-139">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="e80fc-139">Additional resources</span></span>

* [<span data-ttu-id="e80fc-140">Introdução ao ASP.NET</span><span class="sxs-lookup"><span data-stu-id="e80fc-140">Introduction to ASP.NET</span></span>](/aspnet/overview)
* [<span data-ttu-id="e80fc-141">Introdução ao ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e80fc-141">Introduction to ASP.NET Core</span></span>](xref:index)
* <xref:host-and-deploy/azure-apps/index>

> [!NOTE]
> <span data-ttu-id="e80fc-142">Estamos testando a usabilidade de uma nova estrutura proposta para o sumário do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e80fc-142">We’re testing the usability of a proposed new structure for the ASP.NET Core table of contents.</span></span>  <span data-ttu-id="e80fc-143">Se você tiver alguns minutos para experimentar um exercício de localização de sete tópicos diferentes no sumário atual ou proposto, [clique aqui para participar do estudo](https://dpk4xbh5.optimalworkshop.com/treejack/aa11wn82).</span><span class="sxs-lookup"><span data-stu-id="e80fc-143">If you have a few minutes to try an exercise of finding 7 different topics in the current or proposed table of contents, please [click here to participate in the study](https://dpk4xbh5.optimalworkshop.com/treejack/aa11wn82).</span></span>
