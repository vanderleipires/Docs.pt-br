---
title: Introdução a Páginas Razor do ASP.NET Core no Visual Studio Code
author: rick-anderson
description: Conheça as noções básicas da criação de um aplicativo Web Páginas Razor do ASP.NET Core Razor com o Visual Studio Code.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/27/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages-vsc/razor-pages-start
ms.openlocfilehash: 17ab52b80a40f6204e2bf2cf9048071c55c0a708
ms.sourcegitcommit: 63fb07fb3f71b32daf2c9466e132f2e7cc617163
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/10/2018
ms.locfileid: "35252211"
---
# <a name="get-started-with-aspnet-core-razor-pages-in-visual-studio-code"></a><span data-ttu-id="bdd84-103">Introdução a Páginas Razor do ASP.NET Core no Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="bdd84-103">Get started with ASP.NET Core Razor Pages in Visual Studio Code</span></span>

<span data-ttu-id="bdd84-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="bdd84-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="bdd84-105">Este tutorial ensina as noções básicas de criação de um aplicativo Web de Páginas do Razor do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bdd84-105">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="bdd84-106">Recomendamos que você conclua a [Introdução a Páginas do Razor](xref:mvc/razor-pages/index) antes de iniciar este tutorial.</span><span class="sxs-lookup"><span data-stu-id="bdd84-106">We recommend you complete [Introduction to Razor Pages](xref:mvc/razor-pages/index) before starting this tutorial.</span></span> <span data-ttu-id="bdd84-107">Páginas do Razor é a maneira recomendada para criar a interface do usuário para aplicativos Web no ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bdd84-107">Razor Pages is the recommended way to build UI for web applications in ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bdd84-108">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="bdd84-108">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="bdd84-109">Criar um aplicativo Web do Razor</span><span class="sxs-lookup"><span data-stu-id="bdd84-109">Create a Razor web app</span></span>

<span data-ttu-id="bdd84-110">Em um terminal, execute os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="bdd84-110">From a terminal, run the following commands:</span></span>

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new webapp -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

::: moniker-end

<span data-ttu-id="bdd84-112">Os comandos anteriores usam a [CLI do .NET Core](https://docs.microsoft.com/dotnet/core/tools/dotnet) para criar e executar um projeto de Páginas do Razor.</span><span class="sxs-lookup"><span data-stu-id="bdd84-112">The preceding commands use the [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) to create and run a Razor Pages project.</span></span> <span data-ttu-id="bdd84-113">Abra um navegador em http://localhost:5000 para exibir o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="bdd84-113">Open a browser to http://localhost:5000 to view the application.</span></span>

![Página Inicial ou de Índice](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE [razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a><span data-ttu-id="bdd84-115">Abrir o projeto</span><span class="sxs-lookup"><span data-stu-id="bdd84-115">Open the project</span></span>

<span data-ttu-id="bdd84-116">Pressione Ctrl + C para encerrar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="bdd84-116">Press Ctrl+C to shut down the application.</span></span>

<span data-ttu-id="bdd84-117">No Visual Studio Code (VSCode), selecione **Arquivo > Abrir Pasta** e, em seguida, selecione a pasta *RazorPagesMovie*.</span><span class="sxs-lookup"><span data-stu-id="bdd84-117">From Visual Studio Code (VS Code), select **File > Open Folder**, and then select the *RazorPagesMovie* folder.</span></span>

- <span data-ttu-id="bdd84-118">Selecione **Sim** para a mensagem de **Aviso** "Os ativos necessários para criar e depurar estão ausentes no 'RazorPagesMovie'.</span><span class="sxs-lookup"><span data-stu-id="bdd84-118">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'RazorPagesMovie'.</span></span> <span data-ttu-id="bdd84-119">Deseja adicioná-los?"</span><span class="sxs-lookup"><span data-stu-id="bdd84-119">Add them?"</span></span>
- <span data-ttu-id="bdd84-120">Selecione **Restaurar** para a mensagem **Informativa** "Há dependências não resolvidas".</span><span class="sxs-lookup"><span data-stu-id="bdd84-120">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="bdd84-121">Iniciar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="bdd84-121">Launch the app</span></span>

<span data-ttu-id="bdd84-122">Pressione Ctrl+F5 para iniciar o aplicativo sem depuração.</span><span class="sxs-lookup"><span data-stu-id="bdd84-122">Press Ctrl+F5 to start the app without debugging.</span></span> <span data-ttu-id="bdd84-123">Alternativamente, do menu **Depurar**, selecione **Iniciar Sem Depurar**.</span><span class="sxs-lookup"><span data-stu-id="bdd84-123">Alternatively, from the **Debug** menu, select **Start Without Debugging**.</span></span>

<span data-ttu-id="bdd84-124">No próximo tutorial, adicionaremos um modelo para o projeto.</span><span class="sxs-lookup"><span data-stu-id="bdd84-124">In the next tutorial, we add a model to the project.</span></span> 

> [!div class="step-by-step"]
> [<span data-ttu-id="bdd84-125">Próximo: adicionando um modelo</span><span class="sxs-lookup"><span data-stu-id="bdd84-125">Next: Adding a model</span></span>](xref:tutorials/razor-pages-vsc/model)  
