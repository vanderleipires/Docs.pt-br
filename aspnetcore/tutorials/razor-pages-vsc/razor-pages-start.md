---
title: "Introdução a Páginas do Razor no ASP.NET Core com o Visual Studio Code"
author: rick-anderson
description: "Introdução a Páginas do Razor no ASP.NET Core usando o Visual Studio Code"
manager: wpickett
ms.author: riande
ms.date: 08/27/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages-vsc/razor-pages-start
ms.openlocfilehash: 7c01d802e59951281c86c8eab64b7c6b9d646fbf
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2018
---
# <a name="getting-started-with-razor-pages-in-aspnet-core-with-visual-studio-code"></a><span data-ttu-id="54b71-103">Introdução a Páginas do Razor no ASP.NET Core com o Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="54b71-103">Getting started with Razor Pages in ASP.NET Core with Visual Studio Code</span></span>

<span data-ttu-id="54b71-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="54b71-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="54b71-105">Este tutorial ensina as noções básicas de criação de um aplicativo Web de Páginas do Razor do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="54b71-105">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="54b71-106">Recomendamos que você conclua a [Introdução a Páginas do Razor](xref:mvc/razor-pages/index) antes de iniciar este tutorial.</span><span class="sxs-lookup"><span data-stu-id="54b71-106">We recommend you complete [Introduction to Razor Pages](xref:mvc/razor-pages/index) before starting this tutorial.</span></span> <span data-ttu-id="54b71-107">Páginas do Razor é a maneira recomendada para criar a interface do usuário para aplicativos Web no ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="54b71-107">Razor Pages is the recommended way to build UI for web applications in ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="54b71-108">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="54b71-108">Prerequisites</span></span>

<span data-ttu-id="54b71-109">Instale o seguinte:</span><span class="sxs-lookup"><span data-stu-id="54b71-109">Install the following:</span></span>

* <span data-ttu-id="54b71-110">[SDK do .NET Core 2.0.0](https://www.microsoft.com/net/core) ou posterior</span><span class="sxs-lookup"><span data-stu-id="54b71-110">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later</span></span>
* [<span data-ttu-id="54b71-111">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="54b71-111">Visual Studio Code</span></span>](https://code.visualstudio.com)
* <span data-ttu-id="54b71-112">[Extensão C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) do VS Code</span><span class="sxs-lookup"><span data-stu-id="54b71-112">VS Code [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span></span> 

## <a name="create-a-razor-web-app"></a><span data-ttu-id="54b71-113">Criar um aplicativo Web do Razor</span><span class="sxs-lookup"><span data-stu-id="54b71-113">Create a Razor web app</span></span>

<span data-ttu-id="54b71-114">Em um terminal, execute os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="54b71-114">From a terminal, run the following commands:</span></span>

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

<span data-ttu-id="54b71-115">Os comandos anteriores usam a [CLI do .NET Core](https://docs.microsoft.com/dotnet/core/tools/dotnet) para criar e executar um projeto de Páginas do Razor.</span><span class="sxs-lookup"><span data-stu-id="54b71-115">The preceding commands use the [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) to create and run a Razor Pages project.</span></span> <span data-ttu-id="54b71-116">Abra um navegador em http://localhost:5000 para exibir o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="54b71-116">Open a browser to http://localhost:5000 to view the application.</span></span>

![Página Inicial ou de Índice](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a><span data-ttu-id="54b71-118">Abrir o projeto</span><span class="sxs-lookup"><span data-stu-id="54b71-118">Open the project</span></span>

<span data-ttu-id="54b71-119">Pressione Ctrl + C para encerrar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="54b71-119">Press Ctrl+C to shut down the application.</span></span>

<span data-ttu-id="54b71-120">No Visual Studio Code (VSCode), selecione **Arquivo > Abrir Pasta** e, em seguida, selecione a pasta *RazorPagesMovie*.</span><span class="sxs-lookup"><span data-stu-id="54b71-120">From Visual Studio Code (VS Code), select **File > Open Folder**, and then select the *RazorPagesMovie* folder.</span></span>

- <span data-ttu-id="54b71-121">Selecione **Sim** para a mensagem de **Aviso** "Os ativos necessários para criar e depurar estão ausentes no 'RazorPagesMovie'.</span><span class="sxs-lookup"><span data-stu-id="54b71-121">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'RazorPagesMovie'.</span></span> <span data-ttu-id="54b71-122">Deseja adicioná-los?"</span><span class="sxs-lookup"><span data-stu-id="54b71-122">Add them?"</span></span>
- <span data-ttu-id="54b71-123">Selecione **Restaurar** para a mensagem **Informativa** "Há dependências não resolvidas".</span><span class="sxs-lookup"><span data-stu-id="54b71-123">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="54b71-124">Iniciar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="54b71-124">Launch the app</span></span>

<span data-ttu-id="54b71-125">Pressione Ctrl+F5 para iniciar o aplicativo sem depuração.</span><span class="sxs-lookup"><span data-stu-id="54b71-125">Press Ctrl+F5 to start the app without debugging.</span></span> <span data-ttu-id="54b71-126">Alternativamente, do menu **Depurar**, selecione **Iniciar Sem Depurar**.</span><span class="sxs-lookup"><span data-stu-id="54b71-126">Alternatively, from the **Debug** menu, select **Start Without Debugging**.</span></span>

<span data-ttu-id="54b71-127">No próximo tutorial, adicionaremos um modelo para o projeto.</span><span class="sxs-lookup"><span data-stu-id="54b71-127">In the next tutorial, we add a model to the project.</span></span> 

>[!div class="step-by-step"]
[<span data-ttu-id="54b71-128">Próximo: adicionando um modelo</span><span class="sxs-lookup"><span data-stu-id="54b71-128">Next: Adding a model</span></span>](xref:tutorials/razor-pages-vsc/model)  
