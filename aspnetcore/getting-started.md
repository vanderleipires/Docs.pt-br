---
title: "Introdução ao ASP.NET Core 2.0"
author: rick-anderson
description: "Um tutorial rápido que cria e executa um aplicativo simples Olá, Mundo usando o ASP.NET Core."
manager: wpickett
ms.author: riande
ms.date: 10/18/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: getting-started
ms.openlocfilehash: eb1fd748029743ca6991927cc95b612ed1975338
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2018
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="a5be7-103">Introdução ao ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a5be7-103">Get Started with ASP.NET Core</span></span>

> [!NOTE]
> <span data-ttu-id="a5be7-104">Essas instruções referem-se à última versão do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a5be7-104">These instructions are for the latest version of ASP.NET Core.</span></span> <span data-ttu-id="a5be7-105">Deseja começar com uma versão anterior?</span><span class="sxs-lookup"><span data-stu-id="a5be7-105">Looking to get started with an earlier version?</span></span> <span data-ttu-id="a5be7-106">Consulte [a versão 1.1 deste tutorial](xref:getting-started-1.1).</span><span class="sxs-lookup"><span data-stu-id="a5be7-106">See [the 1.1 version of this tutorial](xref:getting-started-1.1).</span></span>

1. <span data-ttu-id="a5be7-107">Instale o [.NET Core](https://www.microsoft.com/net/core/).</span><span class="sxs-lookup"><span data-stu-id="a5be7-107">Install [.NET Core](https://www.microsoft.com/net/core/).</span></span>

2. <span data-ttu-id="a5be7-108">Crie um novo projeto .NET Core.</span><span class="sxs-lookup"><span data-stu-id="a5be7-108">Create a new .NET Core project.</span></span>

   <span data-ttu-id="a5be7-109">No macOS e no Linux, abra uma janela de terminal.</span><span class="sxs-lookup"><span data-stu-id="a5be7-109">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="a5be7-110">No Windows, abra um prompt de comando.</span><span class="sxs-lookup"><span data-stu-id="a5be7-110">On Windows, open a command prompt.</span></span> <span data-ttu-id="a5be7-111">Insira o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="a5be7-111">Enter the following command:</span></span>

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```
    
4. <span data-ttu-id="a5be7-112">Execute o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="a5be7-112">Run the app.</span></span>

    <span data-ttu-id="a5be7-113">Use os seguintes comandos para executar o aplicativo:</span><span class="sxs-lookup"><span data-stu-id="a5be7-113">Use the following commands to run the app:</span></span>

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

5. <span data-ttu-id="a5be7-114">Navegue para [http://localhost:5000](http://localhost:5000)</span><span class="sxs-lookup"><span data-stu-id="a5be7-114">Browse to [http://localhost:5000](http://localhost:5000)</span></span>

6. <span data-ttu-id="a5be7-115">Abra *Pages/About.cshtml* e modifique a página para exibir a mensagem "Olá, mundo!</span><span class="sxs-lookup"><span data-stu-id="a5be7-115">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="a5be7-116">O horário no servidor é @DateTime.Now ":</span><span class="sxs-lookup"><span data-stu-id="a5be7-116">The time on the server is @DateTime.Now ":</span></span>

    [!code-html[Main](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]

7. <span data-ttu-id="a5be7-117">Navegue para [http://localhost:5000/About](http://localhost:5000/About) e verifique as alterações.</span><span class="sxs-lookup"><span data-stu-id="a5be7-117">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

### <a name="next-steps"></a><span data-ttu-id="a5be7-118">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="a5be7-118">Next steps</span></span>

<span data-ttu-id="a5be7-119">Para obter tutoriais de introdução, consulte [Tutoriais do ASP.NET Core](tutorials/index.md)</span><span class="sxs-lookup"><span data-stu-id="a5be7-119">For getting-started tutorials, see [ASP.NET Core Tutorials](tutorials/index.md)</span></span>

<span data-ttu-id="a5be7-120">Para obter uma introdução aos conceitos e à arquitetura do ASP.NET Core, consulte [Introdução ao ASP.NET Core](index.md) e [Conceitos básicos do ASP.NET Core](fundamentals/index.md).</span><span class="sxs-lookup"><span data-stu-id="a5be7-120">For an introduction to ASP.NET Core concepts and architecture, see [ASP.NET Core Introduction](index.md) and [ASP.NET Core Fundamentals](fundamentals/index.md).</span></span>

<span data-ttu-id="a5be7-121">Um aplicativo ASP.NET Core pode usar a Biblioteca de Classes Base e o tempo de execução do .NET Core ou do .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="a5be7-121">An ASP.NET Core app can use the .NET Core or .NET Framework Base Class Library and runtime.</span></span> <span data-ttu-id="a5be7-122">Para obter mais informações, consulte [Escolhendo entre o .NET Core e o .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="a5be7-122">For more information, see [Choosing between .NET Core and .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>
