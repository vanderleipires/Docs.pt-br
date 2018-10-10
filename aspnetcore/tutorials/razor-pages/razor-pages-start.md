---
title: Introdução às Páginas do Razor no ASP.NET Core
author: rick-anderson
description: Esta série de tutoriais mostra como usar Razor Pages no ASP.NET Core. Saiba como criar um modelo, gerar código para Razor Pages, usar o Entity Framework Core e o SQL Server para acesso a dados, adicionar funcionalidade de pesquisa, adicionar validação de entrada e usar migrações para atualizar o modelo.
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 2e1c84a704856a22e1e105f56612194d4bb9c234
ms.sourcegitcommit: 599ebae5c2d6fcb22dfa6ae7d1f4bdfcacb79af4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/26/2018
ms.locfileid: "47210994"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="f2c22-104">Tutorial: introdução ao Razor Pages no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f2c22-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="f2c22-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f2c22-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="f2c22-106">Recomendamos que você siga a versão ASP.NET Core 2.1 deste tutorial.</span><span class="sxs-lookup"><span data-stu-id="f2c22-106">We recommend you follow the ASP.NET Core 2.1 version of this tutorial.</span></span> <span data-ttu-id="f2c22-107">É **muito** mais fácil de seguir e cobre muito mais recursos.</span><span class="sxs-lookup"><span data-stu-id="f2c22-107">It's **much** easier to follow and covers more features.</span></span> <span data-ttu-id="f2c22-108">Selecione **ASP.NET Core 2.1** no seletor de versão.</span><span class="sxs-lookup"><span data-stu-id="f2c22-108">Select **ASP.NET Core 2.1** in the version selector.</span></span>

![Seletor de versão no sumário](razor-pages-start/_static/v21.png)

::: moniker-end

<span data-ttu-id="f2c22-110">Este tutorial ensina as noções básicas de criação de um aplicativo Web de Páginas do Razor do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f2c22-110">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="f2c22-111">As Páginas do Razor são a maneira recomendada para criar a interface do usuário para aplicativos Web no ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f2c22-111">Razor Pages is the recommended way to build UI for web apps in ASP.NET Core.</span></span>

<span data-ttu-id="f2c22-112">Há três versões deste tutorial:</span><span class="sxs-lookup"><span data-stu-id="f2c22-112">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="f2c22-113">Windows: este tutorial</span><span class="sxs-lookup"><span data-stu-id="f2c22-113">Windows: This tutorial</span></span>
* <span data-ttu-id="f2c22-114">MacOS: [Introdução a Páginas Razor com o Visual Studio para Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="f2c22-114">MacOS: [Get started with Razor Pages with Visual Studio for Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span></span>
* <span data-ttu-id="f2c22-115">macOS, Linux e Windows: [Introdução a Páginas Razor no ASP.NET Core no Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="f2c22-115">macOS, Linux, and Windows: [Get started with ASP.NET Core Razor Pages in Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)</span></span>

<span data-ttu-id="f2c22-116">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f2c22-116">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="prerequisites"></a><span data-ttu-id="f2c22-117">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="f2c22-117">Prerequisites</span></span>

[!INCLUDE [Prerequisites](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="f2c22-118">Criar um aplicativo Web do Razor</span><span class="sxs-lookup"><span data-stu-id="f2c22-118">Create a Razor web app</span></span>

* <span data-ttu-id="f2c22-119">No menu **Arquivo** do Visual Studio, selecione **Novo** > **Projeto**.</span><span class="sxs-lookup"><span data-stu-id="f2c22-119">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="f2c22-120">Crie um novo Aplicativo Web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f2c22-120">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="f2c22-121">Nomeie o projeto **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="f2c22-121">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="f2c22-122">É importante nomear o projeto *RazorPagesMovie* de modo que os namespaces façam a correspondência quando você copiar/colar código.</span><span class="sxs-lookup"><span data-stu-id="f2c22-122">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
 <span data-ttu-id="f2c22-123">![novo aplicativo Web ASP.NET Core](razor-pages-start/_static/np_2.1.png)</span><span class="sxs-lookup"><span data-stu-id="f2c22-123">![new ASP.NET Core Web Application](razor-pages-start/_static/np_2.1.png)</span></span>
* <span data-ttu-id="f2c22-124">Selecione **ASP.NET Core 2.1** na lista suspensa e selecione **Aplicativo Web**.</span><span class="sxs-lookup"><span data-stu-id="f2c22-124">Select **ASP.NET Core 2.1** in the dropdown, and then select **Web Application**.</span></span>

 ![novo Aplicativo Web ASP.NET Core](razor-pages-start/_static/np_2_2.1.png)

<span data-ttu-id="f2c22-126">O modelo do Visual Studio cria um projeto inicial:</span><span class="sxs-lookup"><span data-stu-id="f2c22-126">The Visual Studio template creates a starter project:</span></span>

![Gerenciador de Soluções](razor-pages-start/_static/se2.1.png)

<span data-ttu-id="f2c22-128">Pressione **F5** para executar o aplicativo no modo de depuração ou **Ctrl-F5** para executar sem anexar o depurador.</span><span class="sxs-lookup"><span data-stu-id="f2c22-128">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger.</span></span> <span data-ttu-id="f2c22-129">Selecione **Aceitar** para dar consentimento de rastreamento.</span><span class="sxs-lookup"><span data-stu-id="f2c22-129">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="f2c22-130">Este aplicativo não rastreia informações pessoais.</span><span class="sxs-lookup"><span data-stu-id="f2c22-130">This app doesn't track personal information.</span></span> <span data-ttu-id="f2c22-131">O código de modelo gerado inclui ativos para ajudar a cumprir o [RGPD (Regulamento Geral sobre a Proteção de Dados)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="f2c22-131">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

![Página Inicial ou de Índice](razor-pages-start/_static/homeGDPR.png)

<span data-ttu-id="f2c22-133">A imagem a seguir mostra o aplicativo depois de aceitar o rastreamento:</span><span class="sxs-lookup"><span data-stu-id="f2c22-133">The following image shows the app after accepting tracking:</span></span>

![Página Inicial ou de Índice](razor-pages-start/_static/home2.1.png)

* <span data-ttu-id="f2c22-135">O Visual Studio inicia o [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) e executa o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="f2c22-135">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="f2c22-136">A barra de endereços mostra `localhost:port#` e não algo como `example.com`.</span><span class="sxs-lookup"><span data-stu-id="f2c22-136">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="f2c22-137">Isso ocorre porque `localhost` é o nome do host padrão do computador local.</span><span class="sxs-lookup"><span data-stu-id="f2c22-137">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="f2c22-138">Localhost serve somente solicitações da Web do computador local.</span><span class="sxs-lookup"><span data-stu-id="f2c22-138">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="f2c22-139">Quando o Visual Studio cria um projeto Web, uma porta aleatória é usada para o servidor Web.</span><span class="sxs-lookup"><span data-stu-id="f2c22-139">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="f2c22-140">Na imagem anterior, o número da porta é 5001.</span><span class="sxs-lookup"><span data-stu-id="f2c22-140">In the preceding image, the port number is 5001.</span></span> <span data-ttu-id="f2c22-141">Quando você executar o aplicativo, verá um número de porta diferente.</span><span class="sxs-lookup"><span data-stu-id="f2c22-141">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="f2c22-142">Iniciar o aplicativo com **Ctrl+F5** (modo de não depuração) permite que você faça alterações de código, salve o arquivo, atualize o navegador e veja as alterações de código.</span><span class="sxs-lookup"><span data-stu-id="f2c22-142">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="f2c22-143">Muitos desenvolvedores preferem usar o modo de não depuração para iniciar o aplicativo rapidamente e exibir alterações.</span><span class="sxs-lookup"><span data-stu-id="f2c22-143">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE [razor-pages-start](~/includes/RP/2.1/razor-pages-start.md)]

[!INCLUDE [F7](~/includes/RP/F7.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

## <a name="prerequisites"></a><span data-ttu-id="f2c22-144">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="f2c22-144">Prerequisites</span></span>

[!INCLUDE [Prerequisites](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="f2c22-145">Criar um aplicativo Web do Razor</span><span class="sxs-lookup"><span data-stu-id="f2c22-145">Create a Razor web app</span></span>

* <span data-ttu-id="f2c22-146">No menu **Arquivo** do Visual Studio, selecione **Novo** > **Projeto**.</span><span class="sxs-lookup"><span data-stu-id="f2c22-146">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="f2c22-147">Crie um novo Aplicativo Web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f2c22-147">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="f2c22-148">Nomeie o projeto **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="f2c22-148">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="f2c22-149">É importante nomear o projeto *RazorPagesMovie* de modo que os namespaces façam a correspondência quando você copiar/colar código.</span><span class="sxs-lookup"><span data-stu-id="f2c22-149">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
  <span data-ttu-id="f2c22-150">![novo aplicativo Web ASP.NET Core](../../razor-pages/index/_static/np.png)</span><span class="sxs-lookup"><span data-stu-id="f2c22-150">![new ASP.NET Core Web Application](../../razor-pages/index/_static/np.png)</span></span>
* <span data-ttu-id="f2c22-151">Selecione **ASP.NET Core 2.0** na lista suspensa e selecione **Aplicativo Web**.</span><span class="sxs-lookup"><span data-stu-id="f2c22-151">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span>

  [!INCLUDE [install 2.0](~/includes/dotnetcore-on-dotnetfx-vs.md)]

<span data-ttu-id="f2c22-152">O modelo do Visual Studio cria um projeto inicial:</span><span class="sxs-lookup"><span data-stu-id="f2c22-152">The Visual Studio template creates a starter project:</span></span>

![Gerenciador de Soluções](razor-pages-start/_static/se.png)

<span data-ttu-id="f2c22-154">Pressione **F5** para executar o aplicativo no modo de depuração ou **Ctrl-F5** para executar sem anexar o depurador</span><span class="sxs-lookup"><span data-stu-id="f2c22-154">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger</span></span>

![Página Inicial ou de Índice](razor-pages-start/_static/home.png)

* <span data-ttu-id="f2c22-156">O Visual Studio inicia o [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) e executa o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="f2c22-156">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="f2c22-157">A barra de endereços mostra `localhost:port#` e não algo como `example.com`.</span><span class="sxs-lookup"><span data-stu-id="f2c22-157">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="f2c22-158">Isso ocorre porque `localhost` é o nome do host padrão do computador local.</span><span class="sxs-lookup"><span data-stu-id="f2c22-158">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="f2c22-159">Localhost serve somente solicitações da Web do computador local.</span><span class="sxs-lookup"><span data-stu-id="f2c22-159">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="f2c22-160">Quando o Visual Studio cria um projeto Web, uma porta aleatória é usada para o servidor Web.</span><span class="sxs-lookup"><span data-stu-id="f2c22-160">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="f2c22-161">Na imagem anterior, o número da porta é 5000.</span><span class="sxs-lookup"><span data-stu-id="f2c22-161">In the preceding image, the port number is 5000.</span></span> <span data-ttu-id="f2c22-162">Quando você executar o aplicativo, verá um número de porta diferente.</span><span class="sxs-lookup"><span data-stu-id="f2c22-162">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="f2c22-163">Iniciar o aplicativo com **Ctrl+F5** (modo de não depuração) permite que você faça alterações de código, salve o arquivo, atualize o navegador e veja as alterações de código.</span><span class="sxs-lookup"><span data-stu-id="f2c22-163">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="f2c22-164">Muitos desenvolvedores preferem usar o modo de não depuração para iniciar o aplicativo rapidamente e exibir alterações.</span><span class="sxs-lookup"><span data-stu-id="f2c22-164">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE [razor-pages-start](~/includes/RP/razor-pages-start.md)]

::: moniker-end

> [!div class="step-by-step"]
> [<span data-ttu-id="f2c22-165">Próximo: adicionando um modelo</span><span class="sxs-lookup"><span data-stu-id="f2c22-165">Next: Adding a model</span></span>](xref:tutorials/razor-pages/model)
