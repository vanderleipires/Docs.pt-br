---
title: "Introdução ao ASP.NET Core 2.0"
author: rick-anderson
description: "Um tutorial rápido que cria e executa um aplicativo simples Olá, Mundo usando o ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 10/18/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: getting-started
ms.openlocfilehash: b5f1fb0de2776177374b8b4d5ea6041b0fc653a9
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/19/2018
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="ed97e-103">Introdução ao ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ed97e-103">Get Started with ASP.NET Core</span></span>

> [!NOTE]
> <span data-ttu-id="ed97e-104">Essas instruções referem-se à última versão do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ed97e-104">These instructions are for the latest version of ASP.NET Core.</span></span> <span data-ttu-id="ed97e-105">Deseja começar com uma versão anterior?</span><span class="sxs-lookup"><span data-stu-id="ed97e-105">Looking to get started with an earlier version?</span></span> <span data-ttu-id="ed97e-106">Consulte [a versão 1.1 deste tutorial](xref:getting-started-1.1).</span><span class="sxs-lookup"><span data-stu-id="ed97e-106">See [the 1.1 version of this tutorial](xref:getting-started-1.1).</span></span>

1. <span data-ttu-id="ed97e-107">Instale o [.NET Core](https://www.microsoft.com/net/core/).</span><span class="sxs-lookup"><span data-stu-id="ed97e-107">Install [.NET Core](https://www.microsoft.com/net/core/).</span></span>

2. <span data-ttu-id="ed97e-108">Crie um novo projeto .NET Core.</span><span class="sxs-lookup"><span data-stu-id="ed97e-108">Create a new .NET Core project.</span></span>

   <span data-ttu-id="ed97e-109">No macOS e no Linux, abra uma janela de terminal.</span><span class="sxs-lookup"><span data-stu-id="ed97e-109">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="ed97e-110">No Windows, abra um prompt de comando.</span><span class="sxs-lookup"><span data-stu-id="ed97e-110">On Windows, open a command prompt.</span></span> <span data-ttu-id="ed97e-111">Insira o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="ed97e-111">Enter the following command:</span></span>

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```
    
4. <span data-ttu-id="ed97e-112">Execute o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="ed97e-112">Run the app.</span></span>

    <span data-ttu-id="ed97e-113">Use os seguintes comandos para executar o aplicativo:</span><span class="sxs-lookup"><span data-stu-id="ed97e-113">Use the following commands to run the app:</span></span>

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

5. <span data-ttu-id="ed97e-114">Navegue para [http://localhost:5000](http://localhost:5000)</span><span class="sxs-lookup"><span data-stu-id="ed97e-114">Browse to [http://localhost:5000](http://localhost:5000)</span></span>

6. <span data-ttu-id="ed97e-115">Abra *Pages/About.cshtml* e modifique a página para exibir a mensagem "Olá, mundo!</span><span class="sxs-lookup"><span data-stu-id="ed97e-115">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="ed97e-116">O horário no servidor é @DateTime.Now ":</span><span class="sxs-lookup"><span data-stu-id="ed97e-116">The time on the server is @DateTime.Now ":</span></span>

    [!code-html[Main](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]

7. <span data-ttu-id="ed97e-117">Navegue para [http://localhost:5000/About](http://localhost:5000/About) e verifique as alterações.</span><span class="sxs-lookup"><span data-stu-id="ed97e-117">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

### <a name="next-steps"></a><span data-ttu-id="ed97e-118">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="ed97e-118">Next steps</span></span>

<span data-ttu-id="ed97e-119">Para obter tutoriais de introdução, consulte [Tutoriais do ASP.NET Core](tutorials/index.md)</span><span class="sxs-lookup"><span data-stu-id="ed97e-119">For getting-started tutorials, see [ASP.NET Core Tutorials](tutorials/index.md)</span></span>

<span data-ttu-id="ed97e-120">Para obter uma introdução aos conceitos e à arquitetura do ASP.NET Core, consulte [Introdução ao ASP.NET Core](index.md) e [Conceitos básicos do ASP.NET Core](fundamentals/index.md).</span><span class="sxs-lookup"><span data-stu-id="ed97e-120">For an introduction to ASP.NET Core concepts and architecture, see [ASP.NET Core Introduction](index.md) and [ASP.NET Core Fundamentals](fundamentals/index.md).</span></span>

<span data-ttu-id="ed97e-121">Um aplicativo ASP.NET Core pode usar a Biblioteca de Classes Base e o tempo de execução do .NET Core ou do .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="ed97e-121">An ASP.NET Core app can use the .NET Core or .NET Framework Base Class Library and runtime.</span></span> <span data-ttu-id="ed97e-122">Para obter mais informações, consulte [Escolhendo entre o .NET Core e o .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="ed97e-122">For more information, see [Choosing between .NET Core and .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>
