---
title: "Introdução ao ASP.NET Core 1.1"
author: rick-anderson
description: "Um tutorial rápido que cria e executa um aplicativo simples Olá, Mundo usando o ASP.NET Core 1.1."
manager: wpickett
ms.author: riande
ms.date: 08/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: getting-started-1.1
ms.openlocfilehash: 895e91efbba931923540e4cd182862cbc1851585
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2018
---
# <a name="getting-started-with-aspnet-core-11"></a><span data-ttu-id="ea34f-103">Introdução ao ASP.NET Core 1.1</span><span class="sxs-lookup"><span data-stu-id="ea34f-103">Getting Started with ASP.NET Core 1.1</span></span>

> [!NOTE]
> <span data-ttu-id="ea34f-104">Estas instruções referem-se ao ASP.NET Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="ea34f-104">These instructions are for ASP.NET Core 1.1.</span></span> <span data-ttu-id="ea34f-105">Procurando a última versão?</span><span class="sxs-lookup"><span data-stu-id="ea34f-105">Looking for the latest version?</span></span> <span data-ttu-id="ea34f-106">Consulte [a versão atual deste tutorial](xref:getting-started).</span><span class="sxs-lookup"><span data-stu-id="ea34f-106">See [the current version of this tutorial](xref:getting-started).</span></span>

1. <span data-ttu-id="ea34f-107">Instale o **Instalador do SDK** do .NET Core para o SDK 1.0.4 na [página de downloads do SDK 1.0.4 do .NET Core 1.0.5 e 1.1.2](https://github.com/dotnet/core/blob/master/release-notes/download-archives/1.0.5-download.md).</span><span class="sxs-lookup"><span data-stu-id="ea34f-107">Install the .NET Core **SDK Installer** for SDK 1.0.4 from the [.NET Core 1.0.5 & 1.1.2 SDK 1.0.4 downloads page](https://github.com/dotnet/core/blob/master/release-notes/download-archives/1.0.5-download.md).</span></span>

2. <span data-ttu-id="ea34f-108">Crie uma pasta para um novo projeto .NET Core.</span><span class="sxs-lookup"><span data-stu-id="ea34f-108">Create a folder for a new .NET Core project.</span></span>

   <span data-ttu-id="ea34f-109">No macOS e no Linux, abra uma janela de terminal.</span><span class="sxs-lookup"><span data-stu-id="ea34f-109">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="ea34f-110">No Windows, abra um prompt de comando.</span><span class="sxs-lookup"><span data-stu-id="ea34f-110">On Windows, open a command prompt.</span></span>

   ```terminal
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

2. <span data-ttu-id="ea34f-111">Se você instalou uma versão posterior do SDK no computador, crie um arquivo *global.json* para selecionar o SDK 1.0.4.</span><span class="sxs-lookup"><span data-stu-id="ea34f-111">If you have installed a later SDK version on your machine, create a *global.json* file to select the 1.0.4 SDK.</span></span>

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

2. <span data-ttu-id="ea34f-112">Crie um novo projeto .NET Core.</span><span class="sxs-lookup"><span data-stu-id="ea34f-112">Create a new .NET Core project.</span></span>

   ```terminal
   dotnet new web
   ```
   
3.  <span data-ttu-id="ea34f-113">Restaure os pacotes.</span><span class="sxs-lookup"><span data-stu-id="ea34f-113">Restore the packages.</span></span>

    ```terminal
    dotnet restore
    ```

4. <span data-ttu-id="ea34f-114">Execute o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="ea34f-114">Run the app.</span></span>

   <span data-ttu-id="ea34f-115">O comando `dotnet run` compila o aplicativo primeiro, se necessário.</span><span class="sxs-lookup"><span data-stu-id="ea34f-115">The `dotnet run` command builds the app first if needed.</span></span>

   ```terminal
   dotnet run
   ```

5. <span data-ttu-id="ea34f-116">Navegue para `http://localhost:5000`</span><span class="sxs-lookup"><span data-stu-id="ea34f-116">Browse to `http://localhost:5000`</span></span>

<!-- H3 to avoid a single-entry internal TOC -->
### <a name="next-steps"></a><span data-ttu-id="ea34f-117">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="ea34f-117">Next steps</span></span>

<span data-ttu-id="ea34f-118">Para obter tutoriais de introdução, consulte [Tutoriais do ASP.NET Core](tutorials/index.md)</span><span class="sxs-lookup"><span data-stu-id="ea34f-118">For getting-started tutorials, see [ASP.NET Core Tutorials](tutorials/index.md)</span></span>

<span data-ttu-id="ea34f-119">Para obter uma introdução aos conceitos e à arquitetura do ASP.NET Core, consulte [Introdução ao ASP.NET Core](index.md) e [Conceitos básicos do ASP.NET Core](fundamentals/index.md).</span><span class="sxs-lookup"><span data-stu-id="ea34f-119">For an introduction to ASP.NET Core concepts and architecture, see [ASP.NET Core Introduction](index.md) and [ASP.NET Core Fundamentals](fundamentals/index.md).</span></span>

<span data-ttu-id="ea34f-120">Um aplicativo ASP.NET Core pode usar a Biblioteca de Classes Base e o tempo de execução do .NET Core ou do .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="ea34f-120">An ASP.NET Core app can use the .NET Core or .NET Framework Base Class Library and runtime.</span></span> <span data-ttu-id="ea34f-121">Para obter mais informações, consulte [Escolhendo entre o .NET Core e o .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="ea34f-121">For more information, see [Choosing between .NET Core and .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>
