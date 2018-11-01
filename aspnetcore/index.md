---
title: Introdução ao ASP.NET Core
author: rick-anderson
description: Obtenha uma introdução ao ASP.NET Core, uma estrutura de software livre, plataforma cruzada e alto desempenho para a criação de aplicativos modernos conectados à Internet e baseados em nuvem.
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: index
ms.openlocfilehash: e9bca9fe22dbb64086eba3445c50941c5440974c
ms.sourcegitcommit: 4a6bbe84db24c2f3dd2de065de418fde952c8d40
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/30/2018
ms.locfileid: "50253060"
---
# <a name="introduction-to-aspnet-core"></a><span data-ttu-id="b0333-103">Introdução ao ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b0333-103">Introduction to ASP.NET Core</span></span>

<span data-ttu-id="b0333-104">Por [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT) e [Shaun Luttin](https://twitter.com/dicshaunary)</span><span class="sxs-lookup"><span data-stu-id="b0333-104">By [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Shaun Luttin](https://twitter.com/dicshaunary)</span></span>

<span data-ttu-id="b0333-105">O ASP.NET Core é uma estrutura de [software livre](https://github.com/aspnet/home), de multiplaforma e alto desempenho para a criação de aplicativos modernos conectados à Internet e baseados em nuvem.</span><span class="sxs-lookup"><span data-stu-id="b0333-105">ASP.NET Core is a cross-platform, high-performance, [open-source](https://github.com/aspnet/home) framework for building modern, cloud-based, Internet-connected applications.</span></span> <span data-ttu-id="b0333-106">Com o ASP.NET Core, você pode:</span><span class="sxs-lookup"><span data-stu-id="b0333-106">With ASP.NET Core, you can:</span></span>

* <span data-ttu-id="b0333-107">Compilar aplicativos e serviços Web, aplicativos [IoT](https://www.microsoft.com/internet-of-things/) e back-ends móveis.</span><span class="sxs-lookup"><span data-stu-id="b0333-107">Build web apps and services, [IoT](https://www.microsoft.com/internet-of-things/) apps, and mobile backends.</span></span>
* <span data-ttu-id="b0333-108">Usar suas ferramentas de desenvolvimento favoritas no Windows, macOS e Linux.</span><span class="sxs-lookup"><span data-stu-id="b0333-108">Use your favorite development tools on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="b0333-109">Implantar na nuvem ou local.</span><span class="sxs-lookup"><span data-stu-id="b0333-109">Deploy to the cloud or on-premises.</span></span>
* <span data-ttu-id="b0333-110">Executar no [.NET Core ou no .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="b0333-110">Run on [.NET Core or .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="why-use-aspnet-core"></a><span data-ttu-id="b0333-111">Por que usar o ASP.NET Core?</span><span class="sxs-lookup"><span data-stu-id="b0333-111">Why use ASP.NET Core?</span></span>

<span data-ttu-id="b0333-112">Milhões de desenvolvedores usaram (e continuam usando) o [ASP.NET 4.x](/aspnet/overview) para criar aplicativos Web.</span><span class="sxs-lookup"><span data-stu-id="b0333-112">Millions of developers have used (and continue to use) [ASP.NET 4.x](/aspnet/overview) to create web apps.</span></span> <span data-ttu-id="b0333-113">O ASP.NET Core é uma reformulação do ASP.NET 4.x, com alterações de arquitetura que resultam em uma estrutura mais enxuta e modular.</span><span class="sxs-lookup"><span data-stu-id="b0333-113">ASP.NET Core is a redesign of ASP.NET 4.x, with architectural changes that result in a leaner, more modular framework.</span></span>

[!INCLUDE[](~/includes/benefits.md)]

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a><span data-ttu-id="b0333-114">Compilar APIs Web e uma interface do usuário da Web usando o ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="b0333-114">Build web APIs and web UI using ASP.NET Core MVC</span></span>

<span data-ttu-id="b0333-115">O ASP.NET Core MVC fornece recursos que ajudam você a compilar [APIs Web](xref:tutorials/first-web-api) e [aplicativos Web](xref:tutorials/razor-pages/index):</span><span class="sxs-lookup"><span data-stu-id="b0333-115">ASP.NET Core MVC provides features to build [web APIs](xref:tutorials/first-web-api) and [web apps](xref:tutorials/razor-pages/index):</span></span>

* <span data-ttu-id="b0333-116">O [padrão MVC (Model-View-Controller)](xref:mvc/overview) ajuda a tornar as APIs Web e os aplicativos Web testáveis.</span><span class="sxs-lookup"><span data-stu-id="b0333-116">The [Model-View-Controller (MVC) pattern](xref:mvc/overview) helps make your web APIs and web apps testable.</span></span>
* <span data-ttu-id="b0333-117">As [Páginas Razor](xref:razor-pages/index) (novidade no ASP.NET Core 2.0) é um modelo de programação baseado em página que torna a criação da interface do usuário da Web mais fácil e produtiva.</span><span class="sxs-lookup"><span data-stu-id="b0333-117">[Razor Pages](xref:razor-pages/index) (new in ASP.NET Core 2.0) is a page-based programming model that makes building web UI easier and more productive.</span></span>
* <span data-ttu-id="b0333-118">A [marcação Razor](xref:mvc/views/razor) fornece uma sintaxe produtiva para [Páginas Razor](xref:razor-pages/index) e as [Exibições do MVC](xref:mvc/views/overview).</span><span class="sxs-lookup"><span data-stu-id="b0333-118">[Razor markup](xref:mvc/views/razor) provides a productive syntax for [Razor Pages](xref:razor-pages/index) and [MVC views](xref:mvc/views/overview).</span></span>
* <span data-ttu-id="b0333-119">Os [Auxiliares de Marcação](xref:mvc/views/tag-helpers/intro) permitem que o código do servidor participe da criação e renderização de elementos HTML em arquivos do Razor.</span><span class="sxs-lookup"><span data-stu-id="b0333-119">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span>
* <span data-ttu-id="b0333-120">O suporte interno para [vários formatos de dados e negociação de conteúdo](xref:web-api/advanced/formatting) permite que as APIs Web alcancem uma ampla gama de clientes, incluindo navegadores e dispositivos móveis.</span><span class="sxs-lookup"><span data-stu-id="b0333-120">Built-in support for [multiple data formats and content negotiation](xref:web-api/advanced/formatting) lets your web APIs reach a broad range of clients, including browsers and mobile devices.</span></span>
* <span data-ttu-id="b0333-121">O [model binding](xref:mvc/models/model-binding) mapeia automaticamente os dados de solicitações HTTP para os parâmetros de método de ação.</span><span class="sxs-lookup"><span data-stu-id="b0333-121">[Model binding](xref:mvc/models/model-binding) automatically maps data from HTTP requests to action method parameters.</span></span>
* <span data-ttu-id="b0333-122">A [Validação de Modelos](xref:mvc/models/validation) executa automaticamente a validação no lado do cliente e do servidor.</span><span class="sxs-lookup"><span data-stu-id="b0333-122">[Model validation](xref:mvc/models/validation) automatically performs client-side and server-side validation.</span></span>

## <a name="client-side-development"></a><span data-ttu-id="b0333-123">Desenvolvimento do lado do cliente</span><span class="sxs-lookup"><span data-stu-id="b0333-123">Client-side development</span></span>

<span data-ttu-id="b0333-124">ASP.NET Core integra-se perfeitamente com estruturas conhecidas do lado do cliente e bibliotecas, incluindo [Angular](xref:spa/angular), [React](xref:spa/react) e [Bootstrap](https://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="b0333-124">ASP.NET Core integrates seamlessly with popular client-side frameworks and libraries, including [Angular](xref:spa/angular), [React](xref:spa/react), and [Bootstrap](https://getbootstrap.com/).</span></span> <span data-ttu-id="b0333-125">Para saber mais, consulte [Desenvolvimento do lado do cliente](xref:client-side/index).</span><span class="sxs-lookup"><span data-stu-id="b0333-125">For more information, see [Client-side development](xref:client-side/index).</span></span>

<a name="target-framework"></a>

## <a name="aspnet-core-targeting-net-framework"></a><span data-ttu-id="b0333-126">ASP.NET Core direcionado para o .NET Framework</span><span class="sxs-lookup"><span data-stu-id="b0333-126">ASP.NET Core targeting .NET Framework</span></span>

<span data-ttu-id="b0333-127">O ASP.NET Core pode ser direcionado para o .NET Core ou ao .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="b0333-127">ASP.NET Core can target .NET Core or .NET Framework.</span></span> <span data-ttu-id="b0333-128">Os aplicativos do ASP.NET Core direcionados ao .NET Framework não são multiplataforma,&mdash; são executados somente no Windows.</span><span class="sxs-lookup"><span data-stu-id="b0333-128">ASP.NET Core apps targeting .NET Framework aren't cross-platform&mdash;they run on Windows only.</span></span> <span data-ttu-id="b0333-129">Não existem planos para interromper o suporte ao direcionamento para .NET Framework no ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b0333-129">There are no plans to remove support for targeting .NET Framework in ASP.NET Core.</span></span> <span data-ttu-id="b0333-130">Em geral, o ASP.NET Core é composto de bibliotecas do [.NET Standard](/dotnet/standard/net-standard).</span><span class="sxs-lookup"><span data-stu-id="b0333-130">Generally, ASP.NET Core is made up of [.NET Standard](/dotnet/standard/net-standard) libraries.</span></span> <span data-ttu-id="b0333-131">Aplicativos criados com o .NET Standard 2.0 são executados em qualquer lugar com suporte para ele.</span><span class="sxs-lookup"><span data-stu-id="b0333-131">Apps written with .NET Standard 2.0 run anywhere that .NET Standard 2.0 is supported.</span></span>

<span data-ttu-id="b0333-132">O ASP.NET Core 2.x dá suporte para as versões do .NET Framework compatíveis com o .NET Standard 2.0:</span><span class="sxs-lookup"><span data-stu-id="b0333-132">ASP.NET Core 2.x is supported on .NET Framework versions compatible with .NET Standard 2.0:</span></span>

* <span data-ttu-id="b0333-133">O .NET Framework 4.7.1 e versões posteriores são fortemente recomendados.</span><span class="sxs-lookup"><span data-stu-id="b0333-133">.NET Framework 4.7.1 and later is strongly recommended.</span></span>
* <span data-ttu-id="b0333-134">.NET Framework 4.6.1 e versões posteriores.</span><span class="sxs-lookup"><span data-stu-id="b0333-134">.NET Framework 4.6.1 and later.</span></span>

<span data-ttu-id="b0333-135">Há várias vantagens em direcionar para o .NET Core, e essas vantagens aumentam com cada versão.</span><span class="sxs-lookup"><span data-stu-id="b0333-135">There are several advantages to targeting .NET Core, and these advantages increase with each release.</span></span> <span data-ttu-id="b0333-136">Algumas vantagens do .NET Core em relação ao .NET Framework incluem:</span><span class="sxs-lookup"><span data-stu-id="b0333-136">Some advantages of .NET Core over .NET Framework include:</span></span>

* <span data-ttu-id="b0333-137">Multiplataforma.</span><span class="sxs-lookup"><span data-stu-id="b0333-137">Cross-platform.</span></span> <span data-ttu-id="b0333-138">É executado no Windows, no Linux e no macOS.</span><span class="sxs-lookup"><span data-stu-id="b0333-138">Runs on macOS, Linux, and Windows.</span></span>
* <span data-ttu-id="b0333-139">Desempenho aprimorado</span><span class="sxs-lookup"><span data-stu-id="b0333-139">Improved performance</span></span>
* <span data-ttu-id="b0333-140">Controle de versão lado a lado</span><span class="sxs-lookup"><span data-stu-id="b0333-140">Side-by-side versioning</span></span>
* <span data-ttu-id="b0333-141">Novas APIs</span><span class="sxs-lookup"><span data-stu-id="b0333-141">New APIs</span></span>
* <span data-ttu-id="b0333-142">Código Aberto</span><span class="sxs-lookup"><span data-stu-id="b0333-142">Open source</span></span>

<span data-ttu-id="b0333-143">Estamos trabalhando duro para diminuir a diferença de API entre o .NET Framework e o .NET Core.</span><span class="sxs-lookup"><span data-stu-id="b0333-143">We're working hard to close the API gap from .NET Framework to .NET Core.</span></span> <span data-ttu-id="b0333-144">O [Pacote de Compatibilidade do Windows](/dotnet/core/porting/windows-compat-pack) disponibilizou milhares de APIs exclusivas do Windows no .NET Core.</span><span class="sxs-lookup"><span data-stu-id="b0333-144">The [Windows Compatibility Pack](/dotnet/core/porting/windows-compat-pack) made thousands of Windows-only APIs available in .NET Core.</span></span> <span data-ttu-id="b0333-145">Essas APIs não estavam disponíveis no .NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="b0333-145">These APIs weren't available in .NET Core 1.x.</span></span>

## <a name="how-to-download-a-sample"></a><span data-ttu-id="b0333-146">Como baixar uma amostra</span><span class="sxs-lookup"><span data-stu-id="b0333-146">How to download a sample</span></span>

<span data-ttu-id="b0333-147">Muitos dos artigos e tutoriais incluem links para exemplos de código.</span><span class="sxs-lookup"><span data-stu-id="b0333-147">Many of the articles and tutorials include links to sample code.</span></span>

1. <span data-ttu-id="b0333-148">[Baixe o arquivo zip do repositório ASP.NET](https://codeload.github.com/aspnet/Docs/zip/master).</span><span class="sxs-lookup"><span data-stu-id="b0333-148">[Download the ASP.NET repository zip file](https://codeload.github.com/aspnet/Docs/zip/master).</span></span>
1. <span data-ttu-id="b0333-149">Descompacte o arquivo *Docs-master.zip*.</span><span class="sxs-lookup"><span data-stu-id="b0333-149">Unzip the *Docs-master.zip* file.</span></span>
1. <span data-ttu-id="b0333-150">Use a URL no link de exemplo para ajudá-lo a navegar até o diretório de exemplo.</span><span class="sxs-lookup"><span data-stu-id="b0333-150">Use the URL in the sample link to help you navigate to the sample directory.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b0333-151">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="b0333-151">Next steps</span></span>

<span data-ttu-id="b0333-152">Para obter mais informações, consulte os seguintes recursos:</span><span class="sxs-lookup"><span data-stu-id="b0333-152">For more information, see the following resources:</span></span>

* [<span data-ttu-id="b0333-153">Introdução a Páginas do Razor</span><span class="sxs-lookup"><span data-stu-id="b0333-153">Get started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [<span data-ttu-id="b0333-154">Conceitos básicos do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b0333-154">ASP.NET Core fundamentals</span></span>](xref:fundamentals/index)
* <span data-ttu-id="b0333-155">O [Community Standup semanal do ASP.NET](https://live.asp.net/) aborda o progresso e os planos da equipe.</span><span class="sxs-lookup"><span data-stu-id="b0333-155">[The weekly ASP.NET community standup](https://live.asp.net/) covers the team's progress and plans.</span></span> <span data-ttu-id="b0333-156">Ele apresenta o novo software de terceiros e blogs.</span><span class="sxs-lookup"><span data-stu-id="b0333-156">It features new blogs and third-party software.</span></span>
