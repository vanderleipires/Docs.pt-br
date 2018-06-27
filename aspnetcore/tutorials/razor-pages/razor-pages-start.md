---
title: Introdução às Páginas do Razor no ASP.NET Core
author: rick-anderson
description: Conheça as noções básicas da criação de um aplicativo Web Páginas Razor do ASP.NET Core. O aplicativo Páginas Razor é recomendado para cargas de trabalho da Web no ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 5/30/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: d7cdf7c8fac3b2ac1e526c6eeee8205068964ec9
ms.sourcegitcommit: 545ff5a632e2281035c1becec1f99137298e4f5c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34582811"
---
# <a name="get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="fbd64-104">Introdução às Páginas do Razor no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fbd64-104">Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="fbd64-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="fbd64-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="fbd64-106">Recomendamos que você siga a versão ASP.NET Core 2.1 deste tutorial.</span><span class="sxs-lookup"><span data-stu-id="fbd64-106">We recommend you follow the ASP.NET Core 2.1 version of this tutorial.</span></span> <span data-ttu-id="fbd64-107">É **muito** mais fácil de seguir e cobre muito mais recursos.</span><span class="sxs-lookup"><span data-stu-id="fbd64-107">It's **much** easier to follow and covers more features.</span></span> <span data-ttu-id="fbd64-108">Selecione **ASP.NET Core 2.1** no seletor de versão.</span><span class="sxs-lookup"><span data-stu-id="fbd64-108">Select **ASP.NET Core 2.1** in the version selector.</span></span>

![Seletor de versão no sumário](razor-pages-start/_static/v21.png)

::: moniker-end

<span data-ttu-id="fbd64-110">Este tutorial ensina as noções básicas de criação de um aplicativo Web de Páginas do Razor do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="fbd64-110">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="fbd64-111">As Páginas do Razor são a maneira recomendada para criar a interface do usuário para aplicativos Web no ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="fbd64-111">Razor Pages is the recommended way to build UI for web apps in ASP.NET Core.</span></span>

<span data-ttu-id="fbd64-112">Há três versões deste tutorial:</span><span class="sxs-lookup"><span data-stu-id="fbd64-112">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="fbd64-113">Windows: este tutorial</span><span class="sxs-lookup"><span data-stu-id="fbd64-113">Windows: This tutorial</span></span>
* <span data-ttu-id="fbd64-114">MacOS: [Introdução a Páginas Razor com o Visual Studio para Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="fbd64-114">MacOS: [Get started with Razor Pages with Visual Studio for Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span></span>
* <span data-ttu-id="fbd64-115">macOS, Linux e Windows: [Introdução a Páginas Razor no ASP.NET Core no Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="fbd64-115">macOS, Linux, and Windows: [Get started with ASP.NET Core Razor Pages in Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)</span></span>

<span data-ttu-id="fbd64-116">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="fbd64-116">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="prerequisites"></a><span data-ttu-id="fbd64-117">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="fbd64-117">Prerequisites</span></span>

<span data-ttu-id="fbd64-118">[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span><span class="sxs-lookup"><span data-stu-id="fbd64-118">[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span></span>

## <a name="create-a-razor-web-app"></a><span data-ttu-id="fbd64-119">Criar um aplicativo Web do Razor</span><span class="sxs-lookup"><span data-stu-id="fbd64-119">Create a Razor web app</span></span>

* <span data-ttu-id="fbd64-120">No menu **Arquivo** do Visual Studio, selecione **Novo** > **Projeto**.</span><span class="sxs-lookup"><span data-stu-id="fbd64-120">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="fbd64-121">Crie um novo Aplicativo Web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="fbd64-121">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="fbd64-122">Nomeie o projeto **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="fbd64-122">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="fbd64-123">É importante nomear o projeto *RazorPagesMovie* de modo que os namespaces façam a correspondência quando você copiar/colar código.</span><span class="sxs-lookup"><span data-stu-id="fbd64-123">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
 <span data-ttu-id="fbd64-124">![novo aplicativo Web ASP.NET Core](razor-pages-start/_static/np_2.1.png)</span><span class="sxs-lookup"><span data-stu-id="fbd64-124">![new ASP.NET Core Web Application](razor-pages-start/_static/np_2.1.png)</span></span>
* <span data-ttu-id="fbd64-125">Selecione **ASP.NET Core 2.1** na lista suspensa e selecione **Aplicativo Web**.</span><span class="sxs-lookup"><span data-stu-id="fbd64-125">Select **ASP.NET Core 2.1** in the dropdown, and then select **Web Application**.</span></span>

 ![novo Aplicativo Web ASP.NET Core](razor-pages-start/_static/np_2_2.1.png)

<span data-ttu-id="fbd64-127">O modelo do Visual Studio cria um projeto inicial:</span><span class="sxs-lookup"><span data-stu-id="fbd64-127">The Visual Studio template creates a starter project:</span></span>

![Gerenciador de Soluções](razor-pages-start/_static/se2.1.png)

<span data-ttu-id="fbd64-129">Pressione **F5** para executar o aplicativo no modo de depuração ou **Ctrl-F5** para executar sem anexar o depurador.</span><span class="sxs-lookup"><span data-stu-id="fbd64-129">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger.</span></span> <span data-ttu-id="fbd64-130">Selecione **Aceitar** para dar consentimento ao acompanhamento.</span><span class="sxs-lookup"><span data-stu-id="fbd64-130">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="fbd64-131">Este aplicativo não acompanha informações pessoais.</span><span class="sxs-lookup"><span data-stu-id="fbd64-131">This app doesn't track personal information.</span></span> <span data-ttu-id="fbd64-132">O código de modelo gerado inclui ativos para ajudar a cumprir o [RGPD (Regulamento Geral sobre a Proteção de Dados)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="fbd64-132">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

![Página Inicial ou de Índice](razor-pages-start/_static/homeGDPR.png)

<span data-ttu-id="fbd64-134">A imagem a seguir mostra o aplicativo depois de aceitar o acompanhamento:</span><span class="sxs-lookup"><span data-stu-id="fbd64-134">The following image shows the app after accepting tracking:</span></span>

![Página Inicial ou de Índice](razor-pages-start/_static/home2.1.png)

* <span data-ttu-id="fbd64-136">O Visual Studio inicia o [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) e executa o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="fbd64-136">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="fbd64-137">A barra de endereços mostra `localhost:port#` e não algo como `example.com`.</span><span class="sxs-lookup"><span data-stu-id="fbd64-137">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="fbd64-138">Isso ocorre porque `localhost` é o nome do host padrão do computador local.</span><span class="sxs-lookup"><span data-stu-id="fbd64-138">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="fbd64-139">Localhost serve somente solicitações da Web do computador local.</span><span class="sxs-lookup"><span data-stu-id="fbd64-139">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="fbd64-140">Quando o Visual Studio cria um projeto Web, uma porta aleatória é usada para o servidor Web.</span><span class="sxs-lookup"><span data-stu-id="fbd64-140">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="fbd64-141">Na imagem anterior, o número da porta é 5000.</span><span class="sxs-lookup"><span data-stu-id="fbd64-141">In the preceding image, the port number is 5000.</span></span> <span data-ttu-id="fbd64-142">Quando você executar o aplicativo, verá um número de porta diferente.</span><span class="sxs-lookup"><span data-stu-id="fbd64-142">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="fbd64-143">Iniciar o aplicativo com **Ctrl+F5** (modo de não depuração) permite que você faça alterações de código, salve o arquivo, atualize o navegador e veja as alterações de código.</span><span class="sxs-lookup"><span data-stu-id="fbd64-143">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="fbd64-144">Muitos desenvolvedores preferem usar o modo de não depuração para iniciar o aplicativo rapidamente e exibir alterações.</span><span class="sxs-lookup"><span data-stu-id="fbd64-144">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE [razor-pages-start](~/includes/RP/2.1/razor-pages-start.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

## <a name="prerequisites"></a><span data-ttu-id="fbd64-145">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="fbd64-145">Prerequisites</span></span>

<span data-ttu-id="fbd64-146">[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span><span class="sxs-lookup"><span data-stu-id="fbd64-146">[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span></span>

## <a name="create-a-razor-web-app"></a><span data-ttu-id="fbd64-147">Criar um aplicativo Web do Razor</span><span class="sxs-lookup"><span data-stu-id="fbd64-147">Create a Razor web app</span></span>

* <span data-ttu-id="fbd64-148">No menu **Arquivo** do Visual Studio, selecione **Novo** > **Projeto**.</span><span class="sxs-lookup"><span data-stu-id="fbd64-148">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="fbd64-149">Crie um novo Aplicativo Web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="fbd64-149">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="fbd64-150">Nomeie o projeto **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="fbd64-150">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="fbd64-151">É importante nomear o projeto *RazorPagesMovie* de modo que os namespaces façam a correspondência quando você copiar/colar código.</span><span class="sxs-lookup"><span data-stu-id="fbd64-151">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
  <span data-ttu-id="fbd64-152">![novo aplicativo Web ASP.NET Core](../../mvc/razor-pages/index/_static/np.png)</span><span class="sxs-lookup"><span data-stu-id="fbd64-152">![new ASP.NET Core Web Application](../../mvc/razor-pages/index/_static/np.png)</span></span>
* <span data-ttu-id="fbd64-153">Selecione **ASP.NET Core 2.0** na lista suspensa e selecione **Aplicativo Web**.</span><span class="sxs-lookup"><span data-stu-id="fbd64-153">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span>

  [!INCLUDE [install 2.0](~/includes/dotnetcore-on-dotnetfx-vs.md)]

<span data-ttu-id="fbd64-154">O modelo do Visual Studio cria um projeto inicial:</span><span class="sxs-lookup"><span data-stu-id="fbd64-154">The Visual Studio template creates a starter project:</span></span>

![Gerenciador de Soluções](razor-pages-start/_static/se.png)

<span data-ttu-id="fbd64-156">Pressione **F5** para executar o aplicativo no modo de depuração ou **Ctrl-F5** para executar sem anexar o depurador</span><span class="sxs-lookup"><span data-stu-id="fbd64-156">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger</span></span>

![Página Inicial ou de Índice](razor-pages-start/_static/home.png)

* <span data-ttu-id="fbd64-158">O Visual Studio inicia o [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) e executa o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="fbd64-158">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="fbd64-159">A barra de endereços mostra `localhost:port#` e não algo como `example.com`.</span><span class="sxs-lookup"><span data-stu-id="fbd64-159">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="fbd64-160">Isso ocorre porque `localhost` é o nome do host padrão do computador local.</span><span class="sxs-lookup"><span data-stu-id="fbd64-160">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="fbd64-161">Localhost serve somente solicitações da Web do computador local.</span><span class="sxs-lookup"><span data-stu-id="fbd64-161">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="fbd64-162">Quando o Visual Studio cria um projeto Web, uma porta aleatória é usada para o servidor Web.</span><span class="sxs-lookup"><span data-stu-id="fbd64-162">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="fbd64-163">Na imagem anterior, o número da porta é 5000.</span><span class="sxs-lookup"><span data-stu-id="fbd64-163">In the preceding image, the port number is 5000.</span></span> <span data-ttu-id="fbd64-164">Quando você executar o aplicativo, verá um número de porta diferente.</span><span class="sxs-lookup"><span data-stu-id="fbd64-164">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="fbd64-165">Iniciar o aplicativo com **Ctrl+F5** (modo de não depuração) permite que você faça alterações de código, salve o arquivo, atualize o navegador e veja as alterações de código.</span><span class="sxs-lookup"><span data-stu-id="fbd64-165">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="fbd64-166">Muitos desenvolvedores preferem usar o modo de não depuração para iniciar o aplicativo rapidamente e exibir alterações.</span><span class="sxs-lookup"><span data-stu-id="fbd64-166">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE [razor-pages-start](~/includes/RP/2.1/razor-pages-start.md)]

::: moniker-end

> [!div class="step-by-step"]
> [<span data-ttu-id="fbd64-167">Próximo: adicionando um modelo</span><span class="sxs-lookup"><span data-stu-id="fbd64-167">Next: Adding a model</span></span>](xref:tutorials/razor-pages/model)
