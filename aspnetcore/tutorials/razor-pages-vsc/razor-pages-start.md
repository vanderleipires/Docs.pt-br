---
title: Introdução a Páginas Razor do ASP.NET Core no Visual Studio Code
author: rick-anderson
description: Conheça as noções básicas da criação de um aplicativo Web Páginas Razor do ASP.NET Core Razor com o Visual Studio Code.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: tutorials/razor-pages-vsc/razor-pages-start
ms.openlocfilehash: f18d0a8b3ce24c9844b02f8a0b6360f7e1b1bdb7
ms.sourcegitcommit: c43a6f1fe72d7c2db4b5815fd532f2b45d964e07
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/30/2018
ms.locfileid: "50244847"
---
# <a name="get-started-with-aspnet-core-razor-pages-in-visual-studio-code"></a><span data-ttu-id="895b0-103">Introdução a Páginas Razor do ASP.NET Core no Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="895b0-103">Get started with ASP.NET Core Razor Pages in Visual Studio Code</span></span>

<span data-ttu-id="895b0-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="895b0-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="895b0-105">Este tutorial ensina as noções básicas de criação de um aplicativo Web de Páginas do Razor do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="895b0-105">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="895b0-106">Recomendamos que você conclua a [Introdução a Páginas do Razor](xref:razor-pages/index) antes de iniciar este tutorial.</span><span class="sxs-lookup"><span data-stu-id="895b0-106">We recommend you complete [Introduction to Razor Pages](xref:razor-pages/index) before starting this tutorial.</span></span> <span data-ttu-id="895b0-107">Páginas do Razor é a maneira recomendada para criar a interface do usuário para aplicativos Web no ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="895b0-107">Razor Pages is the recommended way to build UI for web applications in ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="895b0-108">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="895b0-108">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="895b0-109">Criar um aplicativo Web do Razor</span><span class="sxs-lookup"><span data-stu-id="895b0-109">Create a Razor web app</span></span>

<span data-ttu-id="895b0-110">Em um terminal, execute os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="895b0-110">From a terminal, run the following commands:</span></span>

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new webapp -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

::: moniker-end

<span data-ttu-id="895b0-111">Os comandos anteriores usam a [CLI do .NET Core](/dotnet/core/tools/dotnet) para criar e executar um projeto de Páginas do Razor.</span><span class="sxs-lookup"><span data-stu-id="895b0-111">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create and run a Razor Pages project.</span></span> <span data-ttu-id="895b0-112">Abra um navegador em http://localhost:5000 para exibir o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="895b0-112">Open a browser to http://localhost:5000 to view the application.</span></span>

![Página Inicial ou de Índice](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE [razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a><span data-ttu-id="895b0-114">Abrir o projeto</span><span class="sxs-lookup"><span data-stu-id="895b0-114">Open the project</span></span>

<span data-ttu-id="895b0-115">Pressione Ctrl + C para encerrar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="895b0-115">Press Ctrl+C to shut down the application.</span></span>

<span data-ttu-id="895b0-116">No Visual Studio Code (VSCode), selecione **Arquivo > Abrir Pasta** e, em seguida, selecione a pasta *RazorPagesMovie*.</span><span class="sxs-lookup"><span data-stu-id="895b0-116">From Visual Studio Code (VS Code), select **File > Open Folder**, and then select the *RazorPagesMovie* folder.</span></span>

- <span data-ttu-id="895b0-117">Selecione **Sim** para a mensagem de **Aviso** "Os ativos necessários para criar e depurar estão ausentes no 'RazorPagesMovie'.</span><span class="sxs-lookup"><span data-stu-id="895b0-117">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'RazorPagesMovie'.</span></span> <span data-ttu-id="895b0-118">Deseja adicioná-los?"</span><span class="sxs-lookup"><span data-stu-id="895b0-118">Add them?"</span></span>
- <span data-ttu-id="895b0-119">Selecione **Restaurar** para a mensagem **Informativa** "Há dependências não resolvidas".</span><span class="sxs-lookup"><span data-stu-id="895b0-119">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="895b0-120">Iniciar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="895b0-120">Launch the app</span></span>

<span data-ttu-id="895b0-121">No menu **Depurar**, selecione **Iniciar sem Depurar**.</span><span class="sxs-lookup"><span data-stu-id="895b0-121">From the **Debug** menu, select **Start Without Debugging**.</span></span> <span data-ttu-id="895b0-122">Como alternativa, é possível pressionar o atalho de teclado exibido ao lado da opção de menu.</span><span class="sxs-lookup"><span data-stu-id="895b0-122">Alternatively, you can press the keyboard shortcut displayed next to the menu option.</span></span> <span data-ttu-id="895b0-123">Este atalho varia dependendo do sistema operacional.</span><span class="sxs-lookup"><span data-stu-id="895b0-123">This shortcut varies depending on your operating system.</span></span>

<span data-ttu-id="895b0-124">No próximo tutorial, adicionaremos um modelo para o projeto.</span><span class="sxs-lookup"><span data-stu-id="895b0-124">In the next tutorial, we add a model to the project.</span></span> 

> [!div class="step-by-step"]
> [<span data-ttu-id="895b0-125">Próximo: adicionando um modelo</span><span class="sxs-lookup"><span data-stu-id="895b0-125">Next: Adding a model</span></span>](xref:tutorials/razor-pages-vsc/model)  
