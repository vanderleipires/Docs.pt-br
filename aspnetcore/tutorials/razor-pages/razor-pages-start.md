---
title: Introdução às Páginas do Razor no ASP.NET Core
author: rick-anderson
description: Conheça as noções básicas da criação de um aplicativo Web Páginas Razor do ASP.NET Core. O aplicativo Páginas Razor é recomendado para cargas de trabalho da Web no ASP.NET Core.
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: e317b49f2ad33c392de33bc32a87f67bb8cb72a0
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278038"
---
# <a name="get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="93ea3-104">Introdução às Páginas do Razor no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="93ea3-104">Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="93ea3-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="93ea3-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="93ea3-106">Recomendamos que você siga a versão ASP.NET Core 2.1 deste tutorial.</span><span class="sxs-lookup"><span data-stu-id="93ea3-106">We recommend you follow the ASP.NET Core 2.1 version of this tutorial.</span></span> <span data-ttu-id="93ea3-107">É **muito** mais fácil de seguir e cobre muito mais recursos.</span><span class="sxs-lookup"><span data-stu-id="93ea3-107">It's **much** easier to follow and covers more features.</span></span> <span data-ttu-id="93ea3-108">Selecione **ASP.NET Core 2.1** no seletor de versão.</span><span class="sxs-lookup"><span data-stu-id="93ea3-108">Select **ASP.NET Core 2.1** in the version selector.</span></span>

![Seletor de versão no sumário](razor-pages-start/_static/v21.png)

::: moniker-end

<span data-ttu-id="93ea3-110">Este tutorial ensina as noções básicas de criação de um aplicativo Web de Páginas do Razor do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="93ea3-110">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="93ea3-111">As Páginas do Razor são a maneira recomendada para criar a interface do usuário para aplicativos Web no ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="93ea3-111">Razor Pages is the recommended way to build UI for web apps in ASP.NET Core.</span></span>

<span data-ttu-id="93ea3-112">Há três versões deste tutorial:</span><span class="sxs-lookup"><span data-stu-id="93ea3-112">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="93ea3-113">Windows: este tutorial</span><span class="sxs-lookup"><span data-stu-id="93ea3-113">Windows: This tutorial</span></span>
* <span data-ttu-id="93ea3-114">MacOS: [Introdução a Páginas Razor com o Visual Studio para Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="93ea3-114">MacOS: [Get started with Razor Pages with Visual Studio for Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span></span>
* <span data-ttu-id="93ea3-115">macOS, Linux e Windows: [Introdução a Páginas Razor no ASP.NET Core no Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="93ea3-115">macOS, Linux, and Windows: [Get started with ASP.NET Core Razor Pages in Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)</span></span>

<span data-ttu-id="93ea3-116">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="93ea3-116">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="prerequisites"></a><span data-ttu-id="93ea3-117">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="93ea3-117">Prerequisites</span></span>

<span data-ttu-id="93ea3-118">[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span><span class="sxs-lookup"><span data-stu-id="93ea3-118">[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span></span>

## <a name="create-a-razor-web-app"></a><span data-ttu-id="93ea3-119">Criar um aplicativo Web do Razor</span><span class="sxs-lookup"><span data-stu-id="93ea3-119">Create a Razor web app</span></span>

* <span data-ttu-id="93ea3-120">No menu **Arquivo** do Visual Studio, selecione **Novo** > **Projeto**.</span><span class="sxs-lookup"><span data-stu-id="93ea3-120">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="93ea3-121">Crie um novo Aplicativo Web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="93ea3-121">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="93ea3-122">Nomeie o projeto **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="93ea3-122">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="93ea3-123">É importante nomear o projeto *RazorPagesMovie* de modo que os namespaces façam a correspondência quando você copiar/colar código.</span><span class="sxs-lookup"><span data-stu-id="93ea3-123">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
 <span data-ttu-id="93ea3-124">![novo aplicativo Web ASP.NET Core](razor-pages-start/_static/np_2.1.png)</span><span class="sxs-lookup"><span data-stu-id="93ea3-124">![new ASP.NET Core Web Application](razor-pages-start/_static/np_2.1.png)</span></span>
* <span data-ttu-id="93ea3-125">Selecione **ASP.NET Core 2.1** na lista suspensa e selecione **Aplicativo Web**.</span><span class="sxs-lookup"><span data-stu-id="93ea3-125">Select **ASP.NET Core 2.1** in the dropdown, and then select **Web Application**.</span></span>

 ![novo Aplicativo Web ASP.NET Core](razor-pages-start/_static/np_2_2.1.png)

<span data-ttu-id="93ea3-127">O modelo do Visual Studio cria um projeto inicial:</span><span class="sxs-lookup"><span data-stu-id="93ea3-127">The Visual Studio template creates a starter project:</span></span>

![Gerenciador de Soluções](razor-pages-start/_static/se2.1.png)

<span data-ttu-id="93ea3-129">Pressione **F5** para executar o aplicativo no modo de depuração ou **Ctrl-F5** para executar sem anexar o depurador.</span><span class="sxs-lookup"><span data-stu-id="93ea3-129">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger.</span></span> <span data-ttu-id="93ea3-130">Selecione **Aceitar** para dar consentimento ao acompanhamento.</span><span class="sxs-lookup"><span data-stu-id="93ea3-130">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="93ea3-131">Este aplicativo não acompanha informações pessoais.</span><span class="sxs-lookup"><span data-stu-id="93ea3-131">This app doesn't track personal information.</span></span> <span data-ttu-id="93ea3-132">O código de modelo gerado inclui ativos para ajudar a cumprir o [RGPD (Regulamento Geral sobre a Proteção de Dados)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="93ea3-132">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

![Página Inicial ou de Índice](razor-pages-start/_static/homeGDPR.png)

<span data-ttu-id="93ea3-134">A imagem a seguir mostra o aplicativo depois de aceitar o acompanhamento:</span><span class="sxs-lookup"><span data-stu-id="93ea3-134">The following image shows the app after accepting tracking:</span></span>

![Página Inicial ou de Índice](razor-pages-start/_static/home2.1.png)

* <span data-ttu-id="93ea3-136">O Visual Studio inicia o [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) e executa o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="93ea3-136">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="93ea3-137">A barra de endereços mostra `localhost:port#` e não algo como `example.com`.</span><span class="sxs-lookup"><span data-stu-id="93ea3-137">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="93ea3-138">Isso ocorre porque `localhost` é o nome do host padrão do computador local.</span><span class="sxs-lookup"><span data-stu-id="93ea3-138">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="93ea3-139">Localhost serve somente solicitações da Web do computador local.</span><span class="sxs-lookup"><span data-stu-id="93ea3-139">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="93ea3-140">Quando o Visual Studio cria um projeto Web, uma porta aleatória é usada para o servidor Web.</span><span class="sxs-lookup"><span data-stu-id="93ea3-140">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="93ea3-141">Na imagem anterior, o número da porta é 5000.</span><span class="sxs-lookup"><span data-stu-id="93ea3-141">In the preceding image, the port number is 5000.</span></span> <span data-ttu-id="93ea3-142">Quando você executar o aplicativo, verá um número de porta diferente.</span><span class="sxs-lookup"><span data-stu-id="93ea3-142">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="93ea3-143">Iniciar o aplicativo com **Ctrl+F5** (modo de não depuração) permite que você faça alterações de código, salve o arquivo, atualize o navegador e veja as alterações de código.</span><span class="sxs-lookup"><span data-stu-id="93ea3-143">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="93ea3-144">Muitos desenvolvedores preferem usar o modo de não depuração para iniciar o aplicativo rapidamente e exibir alterações.</span><span class="sxs-lookup"><span data-stu-id="93ea3-144">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE [razor-pages-start](~/includes/RP/2.1/razor-pages-start.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

## <a name="prerequisites"></a><span data-ttu-id="93ea3-145">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="93ea3-145">Prerequisites</span></span>

<span data-ttu-id="93ea3-146">[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span><span class="sxs-lookup"><span data-stu-id="93ea3-146">[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span></span>

## <a name="create-a-razor-web-app"></a><span data-ttu-id="93ea3-147">Criar um aplicativo Web do Razor</span><span class="sxs-lookup"><span data-stu-id="93ea3-147">Create a Razor web app</span></span>

* <span data-ttu-id="93ea3-148">No menu **Arquivo** do Visual Studio, selecione **Novo** > **Projeto**.</span><span class="sxs-lookup"><span data-stu-id="93ea3-148">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="93ea3-149">Crie um novo Aplicativo Web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="93ea3-149">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="93ea3-150">Nomeie o projeto **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="93ea3-150">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="93ea3-151">É importante nomear o projeto *RazorPagesMovie* de modo que os namespaces façam a correspondência quando você copiar/colar código.</span><span class="sxs-lookup"><span data-stu-id="93ea3-151">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
  <span data-ttu-id="93ea3-152">![novo aplicativo Web ASP.NET Core](../../razor-pages/index/_static/np.png)</span><span class="sxs-lookup"><span data-stu-id="93ea3-152">![new ASP.NET Core Web Application](../../razor-pages/index/_static/np.png)</span></span>
* <span data-ttu-id="93ea3-153">Selecione **ASP.NET Core 2.0** na lista suspensa e selecione **Aplicativo Web**.</span><span class="sxs-lookup"><span data-stu-id="93ea3-153">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span>

  [!INCLUDE [install 2.0](~/includes/dotnetcore-on-dotnetfx-vs.md)]

<span data-ttu-id="93ea3-154">O modelo do Visual Studio cria um projeto inicial:</span><span class="sxs-lookup"><span data-stu-id="93ea3-154">The Visual Studio template creates a starter project:</span></span>

![Gerenciador de Soluções](razor-pages-start/_static/se.png)

<span data-ttu-id="93ea3-156">Pressione **F5** para executar o aplicativo no modo de depuração ou **Ctrl-F5** para executar sem anexar o depurador</span><span class="sxs-lookup"><span data-stu-id="93ea3-156">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger</span></span>

![Página Inicial ou de Índice](razor-pages-start/_static/home.png)

* <span data-ttu-id="93ea3-158">O Visual Studio inicia o [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) e executa o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="93ea3-158">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="93ea3-159">A barra de endereços mostra `localhost:port#` e não algo como `example.com`.</span><span class="sxs-lookup"><span data-stu-id="93ea3-159">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="93ea3-160">Isso ocorre porque `localhost` é o nome do host padrão do computador local.</span><span class="sxs-lookup"><span data-stu-id="93ea3-160">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="93ea3-161">Localhost serve somente solicitações da Web do computador local.</span><span class="sxs-lookup"><span data-stu-id="93ea3-161">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="93ea3-162">Quando o Visual Studio cria um projeto Web, uma porta aleatória é usada para o servidor Web.</span><span class="sxs-lookup"><span data-stu-id="93ea3-162">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="93ea3-163">Na imagem anterior, o número da porta é 5000.</span><span class="sxs-lookup"><span data-stu-id="93ea3-163">In the preceding image, the port number is 5000.</span></span> <span data-ttu-id="93ea3-164">Quando você executar o aplicativo, verá um número de porta diferente.</span><span class="sxs-lookup"><span data-stu-id="93ea3-164">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="93ea3-165">Iniciar o aplicativo com **Ctrl+F5** (modo de não depuração) permite que você faça alterações de código, salve o arquivo, atualize o navegador e veja as alterações de código.</span><span class="sxs-lookup"><span data-stu-id="93ea3-165">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="93ea3-166">Muitos desenvolvedores preferem usar o modo de não depuração para iniciar o aplicativo rapidamente e exibir alterações.</span><span class="sxs-lookup"><span data-stu-id="93ea3-166">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE [razor-pages-start](~/includes/RP/2.1/razor-pages-start.md)]

::: moniker-end

> [!div class="step-by-step"]
> [<span data-ttu-id="93ea3-167">Próximo: adicionando um modelo</span><span class="sxs-lookup"><span data-stu-id="93ea3-167">Next: Adding a model</span></span>](xref:tutorials/razor-pages/model)
