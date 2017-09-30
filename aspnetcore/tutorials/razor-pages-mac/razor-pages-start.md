---
title: "Introdução a Páginas do Razor no ASP.NET Core em Mac"
author: rick-anderson
description: "Introdução a Páginas do Razor no ASP.NET Core usando o Visual Studio para Mac"
keywords: "ASP.NET Core, Páginas do Razor, scaffolding, Entity Framework Core, EF, EF Core, banco de dados, mac, macOS, Visual Studio para Mac"
ms.author: riande
manager: wpickett
ms.date: 07/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages-mac/razor-pages-start
ms.openlocfilehash: 9431e8160011d1485925db5cc4f9f83bf7381e97
ms.sourcegitcommit: 67f54fabbfa4e3942f5bfe1f8a7fdfe4a7a75358
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/19/2017
---
# <a name="getting-started-with-razor-pages-in-aspnet-core-with-visual-studio-for-mac"></a><span data-ttu-id="312dc-104">Introdução a Páginas do Razor no ASP.NET Core com o Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="312dc-104">Getting started with Razor Pages in ASP.NET Core with Visual Studio for Mac</span></span>

<span data-ttu-id="312dc-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="312dc-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="312dc-106">Este tutorial ensina as noções básicas de criação de um aplicativo Web de Páginas do Razor do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="312dc-106">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="312dc-107">Recomendamos que você conclua a [Introdução a Páginas do Razor](xref:mvc/razor-pages/index) antes de iniciar este tutorial.</span><span class="sxs-lookup"><span data-stu-id="312dc-107">We recommend you complete [Introduction to Razor Pages](xref:mvc/razor-pages/index) before starting this tutorial.</span></span> <span data-ttu-id="312dc-108">Páginas do Razor é a maneira recomendada para criar a interface do usuário para aplicativos Web no ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="312dc-108">Razor Pages is the recommended way to build UI for web applications in ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="312dc-109">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="312dc-109">Prerequisites</span></span>

<span data-ttu-id="312dc-110">Instale o seguinte:</span><span class="sxs-lookup"><span data-stu-id="312dc-110">Install the following:</span></span>

* <span data-ttu-id="312dc-111">[SDK do .NET Core 2.0.0](https://www.microsoft.com/net/core) ou posterior</span><span class="sxs-lookup"><span data-stu-id="312dc-111">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later</span></span>
* [<span data-ttu-id="312dc-112">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="312dc-112">Visual Studio for Mac</span></span>](https://www.visualstudio.com/vs/visual-studio-mac/)

## <a name="create-a-razor-web-app"></a><span data-ttu-id="312dc-113">Criar um aplicativo Web do Razor</span><span class="sxs-lookup"><span data-stu-id="312dc-113">Create a Razor web app</span></span>

<span data-ttu-id="312dc-114">Em um terminal, execute os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="312dc-114">From a terminal, run the following commands:</span></span>

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

<span data-ttu-id="312dc-115">Os comandos anteriores usam a [CLI do .NET Core](https://docs.microsoft.com/dotnet/core/tools/dotnet) para criar e executar um projeto de Páginas do Razor.</span><span class="sxs-lookup"><span data-stu-id="312dc-115">The preceding commands use the [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) to create and run a Razor Pages project.</span></span> <span data-ttu-id="312dc-116">Abra um navegador em http://localhost:5000 para exibir o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="312dc-116">Open a browser to http://localhost:5000 to view the application.</span></span>

![Página Inicial ou de Índice](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a><span data-ttu-id="312dc-118">Abrir o projeto</span><span class="sxs-lookup"><span data-stu-id="312dc-118">Open the project</span></span>

<span data-ttu-id="312dc-119">Pressione Ctrl + C para encerrar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="312dc-119">Press Ctrl+C to shut down the application.</span></span>

<span data-ttu-id="312dc-120">No Visual Studio, selecione **Arquivo > Abrir** e, em seguida, selecione o arquivo *RazorPagesMovie.csproj*.</span><span class="sxs-lookup"><span data-stu-id="312dc-120">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="312dc-121">Iniciar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="312dc-121">Launch the app</span></span>

<span data-ttu-id="312dc-122">No Visual Studio, selecione **Executar > Iniciar Sem Depuração** para iniciar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="312dc-122">In Visual Studio, select **Run > Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="312dc-123">O Visual Studio inicia o [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview), inicia um navegador e navega para `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="312dc-123">Visual Studio starts [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview), launches a browser, and navigates to `http://localhost:5000`.</span></span>

<span data-ttu-id="312dc-124">No próximo tutorial, adicionaremos um modelo para o projeto.</span><span class="sxs-lookup"><span data-stu-id="312dc-124">In the next tutorial, we add a model to the project.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="312dc-125">Próximo: adicionando um modelo</span><span class="sxs-lookup"><span data-stu-id="312dc-125">Next: Adding a model</span></span>](xref:tutorials/razor-pages-mac/model)
