---
title: Introdução às Páginas do Razor no ASP.NET Core
author: rick-anderson
description: Conheça as noções básicas da criação de um aplicativo Web Páginas Razor do ASP.NET Core. O aplicativo Páginas Razor é recomendado para cargas de trabalho da Web no ASP.NET Core.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 12/22/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: caf4376c0a02931eeec85e5067a082b37ef9da68
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/03/2018
---
# <a name="get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="52980-104">Introdução às Páginas do Razor no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="52980-104">Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="52980-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="52980-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="52980-106">Este tutorial ensina as noções básicas de criação de um aplicativo Web de Páginas do Razor do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="52980-106">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="52980-107">As Páginas do Razor são a maneira recomendada para criar a interface do usuário para aplicativos Web no ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="52980-107">Razor Pages is the recommended way to build UI for web apps in ASP.NET Core.</span></span>

<span data-ttu-id="52980-108">Há três versões deste tutorial:</span><span class="sxs-lookup"><span data-stu-id="52980-108">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="52980-109">Windows: este tutorial</span><span class="sxs-lookup"><span data-stu-id="52980-109">Windows: This tutorial</span></span>
* <span data-ttu-id="52980-110">MacOS: [Introdução a Páginas Razor com o Visual Studio para Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="52980-110">MacOS: [Get started with Razor Pages with Visual Studio for Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span></span>
* <span data-ttu-id="52980-111">macOS, Linux e Windows: [Introdução a Páginas Razor no ASP.NET Core no Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="52980-111">macOS, Linux, and Windows: [Get started with ASP.NET Core Razor Pages in Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)</span></span>

<span data-ttu-id="52980-112">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="52980-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="52980-113">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="52980-113">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="52980-114">Criar um aplicativo Web do Razor</span><span class="sxs-lookup"><span data-stu-id="52980-114">Create a Razor web app</span></span>

* <span data-ttu-id="52980-115">No menu **Arquivo** do Visual Studio, selecione **Novo** > **Projeto**.</span><span class="sxs-lookup"><span data-stu-id="52980-115">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="52980-116">Crie um novo Aplicativo Web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="52980-116">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="52980-117">Nomeie o projeto **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="52980-117">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="52980-118">É importante nomear o projeto *RazorPagesMovie* de modo que os namespaces façam a correspondência quando você copiar/colar código.</span><span class="sxs-lookup"><span data-stu-id="52980-118">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
  <span data-ttu-id="52980-119">![novo aplicativo Web ASP.NET Core](../../mvc/razor-pages/index/_static/np.png)</span><span class="sxs-lookup"><span data-stu-id="52980-119">![new ASP.NET Core Web Application](../../mvc/razor-pages/index/_static/np.png)</span></span>
* <span data-ttu-id="52980-120">Selecione **ASP.NET Core 2.0** na lista suspensa e selecione **Aplicativo Web**.</span><span class="sxs-lookup"><span data-stu-id="52980-120">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span>

  [!INCLUDE [install 2.0](../../includes/dotnetcore-on-dotnetfx-vs.md)]

<span data-ttu-id="52980-121">O modelo do Visual Studio cria um projeto inicial:</span><span class="sxs-lookup"><span data-stu-id="52980-121">The Visual Studio template creates a starter project:</span></span>

![Gerenciador de Soluções](razor-pages-start/_static/se.png)

<span data-ttu-id="52980-123">Pressione **F5** para executar o aplicativo no modo de depuração ou **Ctrl-F5** para executar sem anexar o depurador</span><span class="sxs-lookup"><span data-stu-id="52980-123">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger</span></span>

![Página Inicial ou de Índice](razor-pages-start/_static/home.png)

* <span data-ttu-id="52980-125">O Visual Studio inicia o [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) e executa o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="52980-125">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="52980-126">A barra de endereços mostra `localhost:port#` e não algo como `example.com`.</span><span class="sxs-lookup"><span data-stu-id="52980-126">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="52980-127">Isso ocorre porque `localhost` é o nome do host padrão do computador local.</span><span class="sxs-lookup"><span data-stu-id="52980-127">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="52980-128">Localhost serve somente solicitações da Web do computador local.</span><span class="sxs-lookup"><span data-stu-id="52980-128">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="52980-129">Quando o Visual Studio cria um projeto Web, uma porta aleatória é usada para o servidor Web.</span><span class="sxs-lookup"><span data-stu-id="52980-129">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="52980-130">Na imagem anterior, o número da porta é 5000.</span><span class="sxs-lookup"><span data-stu-id="52980-130">In the preceding image, the port number is 5000.</span></span> <span data-ttu-id="52980-131">Quando você executar o aplicativo, verá um número de porta diferente.</span><span class="sxs-lookup"><span data-stu-id="52980-131">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="52980-132">Iniciar o aplicativo com **Ctrl+F5** (modo de não depuração) permite que você faça alterações de código, salve o arquivo, atualize o navegador e veja as alterações de código.</span><span class="sxs-lookup"><span data-stu-id="52980-132">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="52980-133">Muitos desenvolvedores preferem usar o modo de não depuração para iniciar o aplicativo rapidamente e exibir alterações.</span><span class="sxs-lookup"><span data-stu-id="52980-133">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE [razor-pages-start](../../includes/RP/razor-pages-start.md)]

> [!div class="step-by-step"]
> [<span data-ttu-id="52980-134">Próximo: adicionando um modelo</span><span class="sxs-lookup"><span data-stu-id="52980-134">Next: Adding a model</span></span>](xref:tutorials/razor-pages/model)
