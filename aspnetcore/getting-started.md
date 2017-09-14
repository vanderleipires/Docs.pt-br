---
title: "Introdução ao ASP.NET Core 2.0"
author: rick-anderson
description: "Um tutorial rápido que cria e executa um aplicativo simples Olá, Mundo usando o ASP.NET Core."
keywords: "ASP.NET Core, tutorial, introdução"
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.assetid: 73543e9d-d9d5-47d6-9664-17a9beea6cd3
ms.technology: aspnet
ms.prod: asp.net-core
uid: getting-started
ms.openlocfilehash: 3399df3958093da9117b013736b1cb370fd6beb2
ms.sourcegitcommit: 297ee5d2f3b3b24eb8a2c4a25195c9e2973cb91b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/14/2017
---
# <a name="getting-started-with-aspnet-core"></a><span data-ttu-id="f16a6-104">Introdução ao ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f16a6-104">Getting Started with ASP.NET Core</span></span>

> [!NOTE]
> <span data-ttu-id="f16a6-105">Essas instruções referem-se à última versão do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f16a6-105">These instructions are for the latest version of ASP.NET Core.</span></span> <span data-ttu-id="f16a6-106">Deseja começar com uma versão anterior?</span><span class="sxs-lookup"><span data-stu-id="f16a6-106">Looking to get started with an earlier version?</span></span> <span data-ttu-id="f16a6-107">Consulte [a versão 1.1 deste tutorial](xref:getting-started-1.1).</span><span class="sxs-lookup"><span data-stu-id="f16a6-107">See [the 1.1 version of this tutorial](xref:getting-started-1.1).</span></span>

1. <span data-ttu-id="f16a6-108">Instale o [.NET Core](https://microsoft.com/net/core/).</span><span class="sxs-lookup"><span data-stu-id="f16a6-108">Install [.NET Core](https://microsoft.com/net/core/).</span></span>

2. <span data-ttu-id="f16a6-109">Crie um novo projeto .NET Core.</span><span class="sxs-lookup"><span data-stu-id="f16a6-109">Create a new .NET Core project.</span></span>

   <span data-ttu-id="f16a6-110">No macOS e no Linux, abra uma janela de terminal.</span><span class="sxs-lookup"><span data-stu-id="f16a6-110">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="f16a6-111">No Windows, abra um prompt de comando.</span><span class="sxs-lookup"><span data-stu-id="f16a6-111">On Windows, open a command prompt.</span></span>

   ```terminal
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   dotnet new web
   ```
    
4. <span data-ttu-id="f16a6-112">Execute o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="f16a6-112">Run the app.</span></span>

   <span data-ttu-id="f16a6-113">O comando `dotnet run` compila o aplicativo primeiro, se necessário.</span><span class="sxs-lookup"><span data-stu-id="f16a6-113">The `dotnet run` command builds the app first if needed.</span></span>

   ```terminal
   dotnet run
   ```

5. <span data-ttu-id="f16a6-114">Navegue para `http://localhost:5000`</span><span class="sxs-lookup"><span data-stu-id="f16a6-114">Browse to `http://localhost:5000`</span></span>

### <a name="next-steps"></a><span data-ttu-id="f16a6-115">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="f16a6-115">Next steps</span></span>

<span data-ttu-id="f16a6-116">Para obter tutoriais de introdução, consulte [Tutoriais do ASP.NET Core](tutorials/index.md)</span><span class="sxs-lookup"><span data-stu-id="f16a6-116">For getting-started tutorials, see [ASP.NET Core Tutorials](tutorials/index.md)</span></span>

<span data-ttu-id="f16a6-117">Para obter uma introdução aos conceitos e à arquitetura do ASP.NET Core, consulte [Introdução ao ASP.NET Core](index.md) e [Conceitos básicos do ASP.NET Core](fundamentals/index.md).</span><span class="sxs-lookup"><span data-stu-id="f16a6-117">For an introduction to ASP.NET Core concepts and architecture, see [ASP.NET Core Introduction](index.md) and [ASP.NET Core Fundamentals](fundamentals/index.md).</span></span>

<span data-ttu-id="f16a6-118">Um aplicativo ASP.NET Core pode usar a Biblioteca de Classes Base e o tempo de execução do .NET Core ou do .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="f16a6-118">An ASP.NET Core app can use the .NET Core or .NET Framework Base Class Library and runtime.</span></span> <span data-ttu-id="f16a6-119">Para obter mais informações, consulte [Escolhendo entre o .NET Core e o .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="f16a6-119">For more information, see [Choosing between .NET Core and .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>
