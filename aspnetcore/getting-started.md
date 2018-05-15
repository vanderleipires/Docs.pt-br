---
title: Introdução ao ASP.NET Core
author: rick-anderson
description: Um tutorial rápido que cria e executa um aplicativo simples Olá, Mundo usando o ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/18/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: getting-started
ms.openlocfilehash: c2f18c69901a5a6503314d508a776e6985872681
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="68e28-103">Introdução ao ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="68e28-103">Get Started with ASP.NET Core</span></span>

> [!NOTE]
> <span data-ttu-id="68e28-104">Essas instruções referem-se à última versão do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="68e28-104">These instructions are for the latest version of ASP.NET Core.</span></span> <span data-ttu-id="68e28-105">Confira [Introdução ao ASP.NET Core 1.1](xref:getting-started-1.1) para obter a versão 1.1 deste documento.</span><span class="sxs-lookup"><span data-stu-id="68e28-105">See [Getting Started with ASP.NET Core 1.1](xref:getting-started-1.1) for the 1.1 version of this document.</span></span>

1. <span data-ttu-id="68e28-106">Instale o [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span><span class="sxs-lookup"><span data-stu-id="68e28-106">Install the [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span></span>

2. <span data-ttu-id="68e28-107">Crie um novo projeto .NET Core.</span><span class="sxs-lookup"><span data-stu-id="68e28-107">Create a new .NET Core project.</span></span>

   <span data-ttu-id="68e28-108">No macOS e no Linux, abra uma janela de terminal.</span><span class="sxs-lookup"><span data-stu-id="68e28-108">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="68e28-109">No Windows, abra um prompt de comando.</span><span class="sxs-lookup"><span data-stu-id="68e28-109">On Windows, open a command prompt.</span></span> <span data-ttu-id="68e28-110">Insira o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="68e28-110">Enter the following command:</span></span>

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```
    
3. <span data-ttu-id="68e28-111">Execute o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="68e28-111">Run the app.</span></span>

    <span data-ttu-id="68e28-112">Use os seguintes comandos para executar o aplicativo:</span><span class="sxs-lookup"><span data-stu-id="68e28-112">Use the following commands to run the app:</span></span>

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

4. <span data-ttu-id="68e28-113">Navegue até [http://localhost:5000](http://localhost:5000)</span><span class="sxs-lookup"><span data-stu-id="68e28-113">Browse to [http://localhost:5000](http://localhost:5000)</span></span>

5. <span data-ttu-id="68e28-114">Abra <em>Pages/About.cshtml</em> e modifique a página para exibir a mensagem "Olá, mundo!</span><span class="sxs-lookup"><span data-stu-id="68e28-114">Open <em>Pages/About.cshtml</em> and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="68e28-115">O horário no servidor é @DateTime.Now ":</span><span class="sxs-lookup"><span data-stu-id="68e28-115">The time on the server is @DateTime.Now ":</span></span>

    [!code-html[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. <span data-ttu-id="68e28-116">Navegue até [ http://localhost:5000/About ](http://localhost:5000/About) e verifique as alterações.</span><span class="sxs-lookup"><span data-stu-id="68e28-116">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

### <a name="next-steps"></a><span data-ttu-id="68e28-117">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="68e28-117">Next steps</span></span>

<span data-ttu-id="68e28-118">Para obter tutoriais de introdução, consulte [Tutoriais do ASP.NET Core](tutorials/index.md)</span><span class="sxs-lookup"><span data-stu-id="68e28-118">For getting-started tutorials, see [ASP.NET Core Tutorials](tutorials/index.md)</span></span>

<span data-ttu-id="68e28-119">Para obter uma introdução aos conceitos e à arquitetura do ASP.NET Core, consulte [Introdução ao ASP.NET Core](index.md) e [Conceitos básicos do ASP.NET Core](fundamentals/index.md).</span><span class="sxs-lookup"><span data-stu-id="68e28-119">For an introduction to ASP.NET Core concepts and architecture, see [ASP.NET Core Introduction](index.md) and [ASP.NET Core Fundamentals](fundamentals/index.md).</span></span>

<span data-ttu-id="68e28-120">Um aplicativo ASP.NET Core pode usar a Biblioteca de Classes Base e o tempo de execução do .NET Core ou do .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="68e28-120">An ASP.NET Core app can use the .NET Core or .NET Framework Base Class Library and runtime.</span></span> <span data-ttu-id="68e28-121">Para obter mais informações, consulte [Escolhendo entre o .NET Core e o .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="68e28-121">For more information, see [Choosing between .NET Core and .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>
