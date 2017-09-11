---
title: "Introdução a Páginas do Razor no ASP.NET Core com o Visual Studio Code"
author: rick-anderson
description: "Introdução a Páginas do Razor no ASP.NET Core usando o Visual Studio Code"
keywords: "ASP.NET Core, Páginas do Razor, scaffolding, Entity Framework Core, EF, EF Core, banco de dados, mac, macOS, Visual Studio Code, Code"
ms.author: riande
manager: wpickett
ms.date: 8/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages-vsc/razor-pages-start
ms.openlocfilehash: aa39de71addb2499af6d322db6da0ec635c54970
ms.sourcegitcommit: d9ec19e5452af83648074db5d96c0a0f4f9e7f9a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/29/2017
---
# <a name="getting-started-with-razor-pages-in-aspnet-core-with-visual-studio-code"></a><span data-ttu-id="a9d0e-104">Introdução a Páginas do Razor no ASP.NET Core com o Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a9d0e-104">Getting started with Razor Pages in ASP.NET Core with Visual Studio Code</span></span>

<span data-ttu-id="a9d0e-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a9d0e-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a9d0e-106">Este tutorial ensina as noções básicas de criação de um aplicativo Web de Páginas do Razor do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a9d0e-106">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="a9d0e-107">Recomendamos que você conclua a [Introdução a Páginas do Razor](xref:mvc/razor-pages/index) antes de iniciar este tutorial.</span><span class="sxs-lookup"><span data-stu-id="a9d0e-107">We recommend you complete [Introduction to Razor Pages](xref:mvc/razor-pages/index) before starting this tutorial.</span></span> <span data-ttu-id="a9d0e-108">Páginas do Razor é a maneira recomendada para criar a interface do usuário para aplicativos Web no ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a9d0e-108">Razor Pages is the recommended way to build UI for web applications in ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a9d0e-109">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="a9d0e-109">Prerequisites</span></span>

<span data-ttu-id="a9d0e-110">Instale o seguinte:</span><span class="sxs-lookup"><span data-stu-id="a9d0e-110">Install the following:</span></span>

* <span data-ttu-id="a9d0e-111">[SDK do .NET Core 2.0.0](https://dot.net/core) ou posterior</span><span class="sxs-lookup"><span data-stu-id="a9d0e-111">[.NET Core 2.0.0 SDK](https://dot.net/core) or later</span></span>
* [<span data-ttu-id="a9d0e-112">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a9d0e-112">Visual Studio Code</span></span>](https://code.visualstudio.com)
* <span data-ttu-id="a9d0e-113">[Extensão C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) do VSCode</span><span class="sxs-lookup"><span data-stu-id="a9d0e-113">VS Code [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span></span> 

## <a name="create-a-razor-web-app"></a><span data-ttu-id="a9d0e-114">Criar um aplicativo Web do Razor</span><span class="sxs-lookup"><span data-stu-id="a9d0e-114">Create a Razor web app</span></span>

<span data-ttu-id="a9d0e-115">Em um terminal, execute os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="a9d0e-115">From a terminal, run the following commands:</span></span>

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

<span data-ttu-id="a9d0e-116">Os comandos anteriores usam a [CLI do .NET Core](https://docs.microsoft.com/dotnet/core/tools/dotnet) para criar e executar um projeto de Páginas do Razor.</span><span class="sxs-lookup"><span data-stu-id="a9d0e-116">The preceding commands use the [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) to create and run a Razor Pages project.</span></span> <span data-ttu-id="a9d0e-117">Abra um navegador em http://localhost:5000 para exibir o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="a9d0e-117">Open a browser to http://localhost:5000 to view the application.</span></span>

![Página Inicial ou de Índice](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a><span data-ttu-id="a9d0e-119">Abrir o projeto</span><span class="sxs-lookup"><span data-stu-id="a9d0e-119">Open the project</span></span>

<span data-ttu-id="a9d0e-120">Pressione Ctrl + C para encerrar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="a9d0e-120">Press Ctrl+C to shut down the application.</span></span>

<span data-ttu-id="a9d0e-121">No Visual Studio Code (VSCode), selecione **Arquivo > Abrir Pasta** e, em seguida, selecione a pasta *RazorPagesMovie*.</span><span class="sxs-lookup"><span data-stu-id="a9d0e-121">From Visual Studio Code (VS Code), select **File > Open Folder**, and then select the *RazorPagesMovie* folder.</span></span>

- <span data-ttu-id="a9d0e-122">Selecione **Sim** para a mensagem de **Aviso** "Os ativos necessários para criar e depurar estão ausentes no 'RazorPagesMovie'.</span><span class="sxs-lookup"><span data-stu-id="a9d0e-122">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'RazorPagesMovie'.</span></span> <span data-ttu-id="a9d0e-123">Deseja adicioná-los?"</span><span class="sxs-lookup"><span data-stu-id="a9d0e-123">Add them?"</span></span>
- <span data-ttu-id="a9d0e-124">Selecione **Restaurar** para a mensagem **Informativa** "Há dependências não resolvidas".</span><span class="sxs-lookup"><span data-stu-id="a9d0e-124">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="a9d0e-125">Iniciar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="a9d0e-125">Launch the app</span></span>

<span data-ttu-id="a9d0e-126">Pressione Ctrl+F5 para iniciar o aplicativo sem depuração.</span><span class="sxs-lookup"><span data-stu-id="a9d0e-126">Press Ctrl+F5 to start the app without debugging.</span></span> <span data-ttu-id="a9d0e-127">Alternativamente, do menu **Depurar**, selecione **Iniciar Sem Depurar**.</span><span class="sxs-lookup"><span data-stu-id="a9d0e-127">Alternatively, from the **Debug** menu, select **Start Without Debugging**.</span></span>

<span data-ttu-id="a9d0e-128">No próximo tutorial, adicionaremos um modelo para o projeto.</span><span class="sxs-lookup"><span data-stu-id="a9d0e-128">In the next tutorial, we add a model to the project.</span></span> 

>[!div class="step-by-step"]
[<span data-ttu-id="a9d0e-129">Próximo: adicionando um modelo</span><span class="sxs-lookup"><span data-stu-id="a9d0e-129">Next: Adding a model</span></span>](xref:tutorials/razor-pages-vsc/model)  
