---
title: "Introdução ao ASP.NET Core 2.0"
author: rick-anderson
description: "Um tutorial rápido que cria e executa um aplicativo simples Olá, Mundo usando o ASP.NET Core."
keywords: "ASP.NET Core, tutorial, introdução"
ms.author: riande
manager: wpickett
ms.date: 10/18/2017
ms.topic: get-started-article
ms.assetid: 73543e9d-d9d5-47d6-9664-17a9beea6cd3
ms.technology: aspnet
ms.prod: asp.net-core
uid: getting-started
ms.openlocfilehash: e944f0f5a3da6d1686ca8a3036666d8dadc9a0f8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="7eead-104">Introdução ao ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7eead-104">Get Started with ASP.NET Core</span></span>

> [!NOTE]
> <span data-ttu-id="7eead-105">Essas instruções referem-se à última versão do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7eead-105">These instructions are for the latest version of ASP.NET Core.</span></span> <span data-ttu-id="7eead-106">Deseja começar com uma versão anterior?</span><span class="sxs-lookup"><span data-stu-id="7eead-106">Looking to get started with an earlier version?</span></span> <span data-ttu-id="7eead-107">Consulte [a versão 1.1 deste tutorial](xref:getting-started-1.1).</span><span class="sxs-lookup"><span data-stu-id="7eead-107">See [the 1.1 version of this tutorial](xref:getting-started-1.1).</span></span>

1. <span data-ttu-id="7eead-108">Instale o [.NET Core](https://www.microsoft.com/net/core/).</span><span class="sxs-lookup"><span data-stu-id="7eead-108">Install [.NET Core](https://www.microsoft.com/net/core/).</span></span>

2. <span data-ttu-id="7eead-109">Crie um novo projeto .NET Core.</span><span class="sxs-lookup"><span data-stu-id="7eead-109">Create a new .NET Core project.</span></span>

   <span data-ttu-id="7eead-110">No macOS e no Linux, abra uma janela de terminal.</span><span class="sxs-lookup"><span data-stu-id="7eead-110">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="7eead-111">No Windows, abra um prompt de comando.</span><span class="sxs-lookup"><span data-stu-id="7eead-111">On Windows, open a command prompt.</span></span>

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```
    
4. <span data-ttu-id="7eead-112">Execute o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="7eead-112">Run the app.</span></span>

    <span data-ttu-id="7eead-113">Use os seguintes comandos para executar o aplicativo:</span><span class="sxs-lookup"><span data-stu-id="7eead-113">Use the following commands to run the app:</span></span>

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

5. <span data-ttu-id="7eead-114">Navegue para [http://localhost:5000](http://localhost:5000)</span><span class="sxs-lookup"><span data-stu-id="7eead-114">Browse to [http://localhost:5000](http://localhost:5000)</span></span>

6. <span data-ttu-id="7eead-115">Abra *Pages/About.cshtml* e modifique a página para exibir a mensagem "Olá, mundo!</span><span class="sxs-lookup"><span data-stu-id="7eead-115">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="7eead-116">O horário no servidor é @DateTime.Now ":</span><span class="sxs-lookup"><span data-stu-id="7eead-116">The time on the server is @DateTime.Now ":</span></span>

    [!code-html[Main](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]

7. <span data-ttu-id="7eead-117">Navegue para [http://localhost:5000/About](http://localhost:5000/About) e verifique as alterações.</span><span class="sxs-lookup"><span data-stu-id="7eead-117">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

### <a name="next-steps"></a><span data-ttu-id="7eead-118">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="7eead-118">Next steps</span></span>

<span data-ttu-id="7eead-119">Para obter tutoriais de introdução, consulte [Tutoriais do ASP.NET Core](tutorials/index.md)</span><span class="sxs-lookup"><span data-stu-id="7eead-119">For getting-started tutorials, see [ASP.NET Core Tutorials](tutorials/index.md)</span></span>

<span data-ttu-id="7eead-120">Para obter uma introdução aos conceitos e à arquitetura do ASP.NET Core, consulte [Introdução ao ASP.NET Core](index.md) e [Conceitos básicos do ASP.NET Core](fundamentals/index.md).</span><span class="sxs-lookup"><span data-stu-id="7eead-120">For an introduction to ASP.NET Core concepts and architecture, see [ASP.NET Core Introduction](index.md) and [ASP.NET Core Fundamentals](fundamentals/index.md).</span></span>

<span data-ttu-id="7eead-121">Um aplicativo ASP.NET Core pode usar a Biblioteca de Classes Base e o tempo de execução do .NET Core ou do .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="7eead-121">An ASP.NET Core app can use the .NET Core or .NET Framework Base Class Library and runtime.</span></span> <span data-ttu-id="7eead-122">Para obter mais informações, consulte [Escolhendo entre o .NET Core e o .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="7eead-122">For more information, see [Choosing between .NET Core and .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>
