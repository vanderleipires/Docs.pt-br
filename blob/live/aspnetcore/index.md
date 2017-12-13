---
title: "Introdução ao ASP.NET Core"
author: rick-anderson
description: Apresenta o ASP.NET Core.
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 09/03/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: index
ms.openlocfilehash: a075c63fddb9e28a1da37d4ef6647808a0dcb583
ms.sourcegitcommit: 8f42ab93402c1b8044815e1e48d0bb84c81f8b59
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/29/2017
---
# <a name="introduction-to-aspnet-core"></a><span data-ttu-id="560e0-104">Introdução ao ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="560e0-104">Introduction to ASP.NET Core</span></span>

<span data-ttu-id="560e0-105">Por [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT) e [Shaun Luttin](https://twitter.com/dicshaunary)</span><span class="sxs-lookup"><span data-stu-id="560e0-105">By [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Shaun Luttin](https://twitter.com/dicshaunary)</span></span>

<span data-ttu-id="560e0-106">O ASP.NET Core é uma estrutura de [software livre](https://github.com/aspnet/home), de multiplaforma e alto desempenho para a criação de aplicativos modernos conectados à Internet e baseados em nuvem.</span><span class="sxs-lookup"><span data-stu-id="560e0-106">ASP.NET Core is a cross-platform, high-performance, [open-source](https://github.com/aspnet/home) framework for building modern, cloud-based, Internet-connected applications.</span></span> <span data-ttu-id="560e0-107">Com o ASP.NET Core, você pode:</span><span class="sxs-lookup"><span data-stu-id="560e0-107">With ASP.NET Core, you can:</span></span>

* <span data-ttu-id="560e0-108">Compilar aplicativos e serviços Web, aplicativos [IoT](https://www.microsoft.com/en-us/internet-of-things/) e back-ends móveis.</span><span class="sxs-lookup"><span data-stu-id="560e0-108">Build web apps and services, [IoT](https://www.microsoft.com/en-us/internet-of-things/) apps, and mobile backends.</span></span>
* <span data-ttu-id="560e0-109">Usar suas ferramentas de desenvolvimento favoritas no Windows, macOS e Linux.</span><span class="sxs-lookup"><span data-stu-id="560e0-109">Use your favorite development tools on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="560e0-110">Implantar na nuvem ou local</span><span class="sxs-lookup"><span data-stu-id="560e0-110">Deploy to the cloud or on-premises</span></span>
* <span data-ttu-id="560e0-111">Executar no [.NET Core ou no .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="560e0-111">Run on [.NET Core or .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="why-use-aspnet-core"></a><span data-ttu-id="560e0-112">Por que usar o ASP.NET Core?</span><span class="sxs-lookup"><span data-stu-id="560e0-112">Why use ASP.NET Core?</span></span>

<span data-ttu-id="560e0-113">Milhões de desenvolvedores usaram (e continuam usando) o ASP.NET para criar aplicativos Web.</span><span class="sxs-lookup"><span data-stu-id="560e0-113">Millions of developers have used (and continue to use) ASP.NET to create web apps.</span></span> <span data-ttu-id="560e0-114">O ASP.NET Core é uma reformulação do ASP.NET, com alterações de arquitetura que resultam em uma estrutura mais enxuta e modular.</span><span class="sxs-lookup"><span data-stu-id="560e0-114">ASP.NET Core is a redesign of ASP.NET, with architectural changes that result in a leaner and modular framework.</span></span>

<span data-ttu-id="560e0-115">O ASP.NET Core oferece os seguintes benefícios:</span><span class="sxs-lookup"><span data-stu-id="560e0-115">ASP.NET Core provides the following benefits:</span></span>

* <span data-ttu-id="560e0-116">Uma história unificada para a criação da interface do usuário da Web e das APIs Web.</span><span class="sxs-lookup"><span data-stu-id="560e0-116">A unified story for building web UI and web APIs.</span></span>
* <span data-ttu-id="560e0-117">Integração de [modernas estruturas do lado do cliente](xref:client-side/index) e fluxos de trabalho de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="560e0-117">Integration of [modern client-side frameworks](xref:client-side/index) and development workflows.</span></span>
* <span data-ttu-id="560e0-118">Um [sistema de configuração](xref:fundamentals/configuration/index) pronto para a nuvem, baseado no ambiente.</span><span class="sxs-lookup"><span data-stu-id="560e0-118">A cloud-ready, environment-based [configuration system](xref:fundamentals/configuration/index).</span></span>
* <span data-ttu-id="560e0-119">[Injeção de dependência](xref:fundamentals/dependency-injection) interna.</span><span class="sxs-lookup"><span data-stu-id="560e0-119">Built-in [dependency injection](xref:fundamentals/dependency-injection).</span></span>
* <span data-ttu-id="560e0-120">Um pipeline de solicitação HTTP leve, modular e de alto desempenho.</span><span class="sxs-lookup"><span data-stu-id="560e0-120">A lightweight, high-performance, and modular HTTP request pipeline.</span></span>
* <span data-ttu-id="560e0-121">A capacidade de hospedar no IIS ou hospedar automaticamente em seu próprio processo.</span><span class="sxs-lookup"><span data-stu-id="560e0-121">Ability to host on IIS or self-host in your own process.</span></span>
* <span data-ttu-id="560e0-122">Pode ser executado no [.NET Core](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server), que dá suporte ao verdadeiro controle de versão do aplicativo lado a lado.</span><span class="sxs-lookup"><span data-stu-id="560e0-122">Can run on [.NET Core](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server), which supports true side-by-side app versioning.</span></span>
* <span data-ttu-id="560e0-123">Ferramentas que simplificam o moderno desenvolvimento para a Web.</span><span class="sxs-lookup"><span data-stu-id="560e0-123">Tooling that simplifies modern web development.</span></span>
* <span data-ttu-id="560e0-124">A capacidade de criar e executar no Windows, macOS e Linux.</span><span class="sxs-lookup"><span data-stu-id="560e0-124">Ability to build and run on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="560e0-125">De software livre e voltado para a comunidade.</span><span class="sxs-lookup"><span data-stu-id="560e0-125">Open-source and community-focused.</span></span>

<span data-ttu-id="560e0-126">O ASP.NET Core é fornecido inteiramente como pacotes [NuGet](https://www.nuget.org/).</span><span class="sxs-lookup"><span data-stu-id="560e0-126">ASP.NET Core ships entirely as [NuGet](https://www.nuget.org/) packages.</span></span> <span data-ttu-id="560e0-127">Isso permite otimizar o aplicativo para incluir apenas os pacotes NuGet necessários.</span><span class="sxs-lookup"><span data-stu-id="560e0-127">This allows you to optimize your app to include just the NuGet packages you need.</span></span> <span data-ttu-id="560e0-128">Os benefícios de uma área de superfície menor do aplicativo incluem maior segurança, manutenção reduzida e desempenho aprimorado.</span><span class="sxs-lookup"><span data-stu-id="560e0-128">The benefits of a smaller app surface area include tighter security, reduced servicing, and improved performance.</span></span>

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a><span data-ttu-id="560e0-129">Compilar APIs Web e uma interface do usuário da Web usando o ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="560e0-129">Build web APIs and web UI using ASP.NET Core MVC</span></span>

<span data-ttu-id="560e0-130">O ASP.NET Core MVC fornece recursos que ajudam você a criar [APIs Web](xref:tutorials/index#building-web-apis) e [aplicativos Web](xref:tutorials/index#building-web-applications):</span><span class="sxs-lookup"><span data-stu-id="560e0-130">ASP.NET Core MVC provides features that help you build [web APIs](xref:tutorials/index#building-web-apis) and [web apps](xref:tutorials/index#building-web-applications):</span></span>

* <span data-ttu-id="560e0-131">O [padrão MVC (Model-View-Controller)](xref:mvc/overview) ajuda a tornar as APIs Web e os aplicativos Web [testáveis](testing/index.md).</span><span class="sxs-lookup"><span data-stu-id="560e0-131">The [Model-View-Controller (MVC) pattern](xref:mvc/overview) helps make your web APIs and web apps [testable](testing/index.md).</span></span>
* <span data-ttu-id="560e0-132">As [Páginas Razor](xref:mvc/razor-pages/index) (novidade no 2.0) é um modelo de programação baseado em página que torna a criação da interface do usuário da Web mais fácil e produtiva.</span><span class="sxs-lookup"><span data-stu-id="560e0-132">[Razor Pages](xref:mvc/razor-pages/index) (new in 2.0) is a page-based programming model that makes building web UI easier and more productive.</span></span>
* <span data-ttu-id="560e0-133">A [sintaxe Razor](xref:mvc/views/razor) fornece uma linguagem produtiva para as [Páginas Razor](xref:mvc/razor-pages/index) e as [Exibições do MVC](xref:mvc/views/overview).</span><span class="sxs-lookup"><span data-stu-id="560e0-133">[Razor syntax](xref:mvc/views/razor) provides a productive language for [Razor Pages](xref:mvc/razor-pages/index) and [MVC Views](xref:mvc/views/overview).</span></span>
* <span data-ttu-id="560e0-134">Os [Auxiliares de Marcação](xref:mvc/views/tag-helpers/intro) permitem que o código do servidor participe da criação e renderização de elementos HTML em arquivos do Razor.</span><span class="sxs-lookup"><span data-stu-id="560e0-134">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span>
* <span data-ttu-id="560e0-135">O suporte interno para [vários formatos de dados e negociação de conteúdo](mvc/models/formatting.md) permite que as APIs Web alcancem uma ampla gama de clientes, incluindo navegadores e dispositivos móveis.</span><span class="sxs-lookup"><span data-stu-id="560e0-135">Built-in support for [multiple data formats and content negotiation](mvc/models/formatting.md) lets your web APIs reach a broad range of clients, including browsers and mobile devices.</span></span>
* <span data-ttu-id="560e0-136">A [Associação de Modelos](xref:mvc/models/model-binding) mapeia automaticamente os dados de solicitações HTTP para os parâmetros de método de ação.</span><span class="sxs-lookup"><span data-stu-id="560e0-136">[Model Binding](xref:mvc/models/model-binding) automatically maps data from HTTP requests to action method parameters.</span></span>
* <span data-ttu-id="560e0-137">A [Validação de Modelos](xref:mvc/models/validation) executa automaticamente a validação do cliente e do servidor.</span><span class="sxs-lookup"><span data-stu-id="560e0-137">[Model Validation](xref:mvc/models/validation) automatically performs client and server-side validation.</span></span>

## <a name="client-side-development"></a><span data-ttu-id="560e0-138">Desenvolvimento do lado do cliente</span><span class="sxs-lookup"><span data-stu-id="560e0-138">Client-side development</span></span>

<span data-ttu-id="560e0-139">O ASP.NET Core foi projetado para ser integrado perfeitamente a uma variedade de estruturas do lado do cliente, incluindo [AngularJS](xref:client-side/angular), [KnockoutJS](xref:client-side/knockout) e [Bootstrap](xref:client-side/bootstrap).</span><span class="sxs-lookup"><span data-stu-id="560e0-139">ASP.NET Core is designed to integrate seamlessly with a variety of client-side frameworks, including [AngularJS](xref:client-side/angular), [KnockoutJS](xref:client-side/knockout), and [Bootstrap](xref:client-side/bootstrap).</span></span> <span data-ttu-id="560e0-140">Consulte [Desenvolvimento do cliente](client-side/index.md) para obter mais detalhes.</span><span class="sxs-lookup"><span data-stu-id="560e0-140">See [Client-side development](client-side/index.md) for more details.</span></span>

## <a name="next-steps"></a><span data-ttu-id="560e0-141">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="560e0-141">Next steps</span></span>

<span data-ttu-id="560e0-142">Para obter mais informações, consulte os seguintes recursos:</span><span class="sxs-lookup"><span data-stu-id="560e0-142">For more information, see the following resources:</span></span>

* [<span data-ttu-id="560e0-143">Tutoriais do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="560e0-143">ASP.NET Core tutorials</span></span>](xref:tutorials/index)
* [<span data-ttu-id="560e0-144">Conceitos básicos do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="560e0-144">ASP.NET Core fundamentals</span></span>](xref:fundamentals/index)
* <span data-ttu-id="560e0-145">O [Community Standup semanal do ASP.NET](https://live.asp.net/) aborda o progresso e os planos da equipe e apresenta novos blogs e software de terceiros.</span><span class="sxs-lookup"><span data-stu-id="560e0-145">[The weekly ASP.NET community standup](https://live.asp.net/) covers the team's progress and plans and features new blogs and third-party software.</span></span>
