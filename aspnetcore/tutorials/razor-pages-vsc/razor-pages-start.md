---
title: "Introdução a Páginas do Razor no ASP.NET Core com o Visual Studio Code"
author: rick-anderson
description: "Introdução a Páginas do Razor no ASP.NET Core usando o Visual Studio Code"
ms.author: riande
manager: wpickett
ms.date: 08/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages-vsc/razor-pages-start
ms.openlocfilehash: 727c3d5c8bed0aef3094816b7e5f0a940aea654c
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/19/2018
---
# <a name="getting-started-with-razor-pages-in-aspnet-core-with-visual-studio-code"></a><span data-ttu-id="d06f1-103">Introdução a Páginas do Razor no ASP.NET Core com o Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d06f1-103">Getting started with Razor Pages in ASP.NET Core with Visual Studio Code</span></span>

<span data-ttu-id="d06f1-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d06f1-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="d06f1-105">Este tutorial ensina as noções básicas de criação de um aplicativo Web de Páginas do Razor do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d06f1-105">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="d06f1-106">Recomendamos que você conclua a [Introdução a Páginas do Razor](xref:mvc/razor-pages/index) antes de iniciar este tutorial.</span><span class="sxs-lookup"><span data-stu-id="d06f1-106">We recommend you complete [Introduction to Razor Pages](xref:mvc/razor-pages/index) before starting this tutorial.</span></span> <span data-ttu-id="d06f1-107">Páginas do Razor é a maneira recomendada para criar a interface do usuário para aplicativos Web no ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d06f1-107">Razor Pages is the recommended way to build UI for web applications in ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d06f1-108">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="d06f1-108">Prerequisites</span></span>

<span data-ttu-id="d06f1-109">Instale o seguinte:</span><span class="sxs-lookup"><span data-stu-id="d06f1-109">Install the following:</span></span>

* <span data-ttu-id="d06f1-110">[SDK do .NET Core 2.0.0](https://www.microsoft.com/net/core) ou posterior</span><span class="sxs-lookup"><span data-stu-id="d06f1-110">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later</span></span>
* [<span data-ttu-id="d06f1-111">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d06f1-111">Visual Studio Code</span></span>](https://code.visualstudio.com)
* <span data-ttu-id="d06f1-112">[Extensão C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) do VS Code</span><span class="sxs-lookup"><span data-stu-id="d06f1-112">VS Code [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span></span> 

## <a name="create-a-razor-web-app"></a><span data-ttu-id="d06f1-113">Criar um aplicativo Web do Razor</span><span class="sxs-lookup"><span data-stu-id="d06f1-113">Create a Razor web app</span></span>

<span data-ttu-id="d06f1-114">Em um terminal, execute os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="d06f1-114">From a terminal, run the following commands:</span></span>

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

<span data-ttu-id="d06f1-115">Os comandos anteriores usam a [CLI do .NET Core](https://docs.microsoft.com/dotnet/core/tools/dotnet) para criar e executar um projeto de Páginas do Razor.</span><span class="sxs-lookup"><span data-stu-id="d06f1-115">The preceding commands use the [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) to create and run a Razor Pages project.</span></span> <span data-ttu-id="d06f1-116">Abra um navegador em http://localhost:5000 para exibir o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="d06f1-116">Open a browser to http://localhost:5000 to view the application.</span></span>

![Página Inicial ou de Índice](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a><span data-ttu-id="d06f1-118">Abrir o projeto</span><span class="sxs-lookup"><span data-stu-id="d06f1-118">Open the project</span></span>

<span data-ttu-id="d06f1-119">Pressione Ctrl + C para encerrar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="d06f1-119">Press Ctrl+C to shut down the application.</span></span>

<span data-ttu-id="d06f1-120">No Visual Studio Code (VSCode), selecione **Arquivo > Abrir Pasta** e, em seguida, selecione a pasta *RazorPagesMovie*.</span><span class="sxs-lookup"><span data-stu-id="d06f1-120">From Visual Studio Code (VS Code), select **File > Open Folder**, and then select the *RazorPagesMovie* folder.</span></span>

- <span data-ttu-id="d06f1-121">Selecione **Sim** para a mensagem de **Aviso** "Os ativos necessários para criar e depurar estão ausentes no 'RazorPagesMovie'.</span><span class="sxs-lookup"><span data-stu-id="d06f1-121">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'RazorPagesMovie'.</span></span> <span data-ttu-id="d06f1-122">Deseja adicioná-los?"</span><span class="sxs-lookup"><span data-stu-id="d06f1-122">Add them?"</span></span>
- <span data-ttu-id="d06f1-123">Selecione **Restaurar** para a mensagem **Informativa** "Há dependências não resolvidas".</span><span class="sxs-lookup"><span data-stu-id="d06f1-123">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="d06f1-124">Iniciar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="d06f1-124">Launch the app</span></span>

<span data-ttu-id="d06f1-125">Pressione Ctrl+F5 para iniciar o aplicativo sem depuração.</span><span class="sxs-lookup"><span data-stu-id="d06f1-125">Press Ctrl+F5 to start the app without debugging.</span></span> <span data-ttu-id="d06f1-126">Alternativamente, do menu **Depurar**, selecione **Iniciar Sem Depurar**.</span><span class="sxs-lookup"><span data-stu-id="d06f1-126">Alternatively, from the **Debug** menu, select **Start Without Debugging**.</span></span>

<span data-ttu-id="d06f1-127">No próximo tutorial, adicionaremos um modelo para o projeto.</span><span class="sxs-lookup"><span data-stu-id="d06f1-127">In the next tutorial, we add a model to the project.</span></span> 

>[!div class="step-by-step"]
[<span data-ttu-id="d06f1-128">Próximo: adicionando um modelo</span><span class="sxs-lookup"><span data-stu-id="d06f1-128">Next: Adding a model</span></span>](xref:tutorials/razor-pages-vsc/model)  
