---
title: "Introdução ao ASP.NET Core MVC e ao Visual Studio para Mac"
author: rick-anderson
description: "Introdução ao ASP.NET Core MVC e ao Visual Studio"
ms.author: riande
manager: wpickett
ms.date: 8/23/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-mac/start-mvc
ms.openlocfilehash: 51c0477c40885d3aaaa7562f8baba0a94cb4f920
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/19/2018
---
# <a name="getting-started-with-aspnet-core-mvc-and-visual-studio-for-mac"></a><span data-ttu-id="4dfdd-103">Introdução ao ASP.NET Core MVC e ao Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="4dfdd-103">Getting started with ASP.NET Core MVC and Visual Studio for Mac</span></span>

<span data-ttu-id="4dfdd-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="4dfdd-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="4dfdd-105">Este tutorial ensina os conceitos básicos da criação de um aplicativo Web ASP.NET Core MVC usando o [Visual Studio para Mac](https://www.visualstudio.com/vs/visual-studio-mac/).</span><span class="sxs-lookup"><span data-stu-id="4dfdd-105">This tutorial teaches you the basics of building an ASP.NET Core MVC web app using [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/).</span></span> 

[!INCLUDE[consider RP](../../includes/razor.md)]

<span data-ttu-id="4dfdd-106">Há três versões deste tutorial:</span><span class="sxs-lookup"><span data-stu-id="4dfdd-106">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="4dfdd-107">macOS: [Compilar um aplicativo ASP.NET Core MVC com o Visual Studio para Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="4dfdd-107">macOS: [Build an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="4dfdd-108">Windows: [Compilar um aplicativo ASP.NET Core MVC com o Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="4dfdd-108">Windows: [Build an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="4dfdd-109">Linux, macOS e Windows: [Compilar um aplicativo ASP.NET Core MVC com o Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="4dfdd-109">Linux, macOS, and Windows: [Build an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4dfdd-110">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="4dfdd-110">Prerequisites</span></span>

<span data-ttu-id="4dfdd-111">Este tutorial exige o [SDK do .NET Core 2.0.0](https://www.microsoft.com/net/core) ou posterior.</span><span class="sxs-lookup"><span data-stu-id="4dfdd-111">This tutorial requires the [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span>

<span data-ttu-id="4dfdd-112">Instale o seguinte:</span><span class="sxs-lookup"><span data-stu-id="4dfdd-112">Install the following:</span></span>

- <span data-ttu-id="4dfdd-113">[SDK do .NET Core 2.0.0](https://www.microsoft.com/net/core) ou posterior</span><span class="sxs-lookup"><span data-stu-id="4dfdd-113">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later</span></span>
- [<span data-ttu-id="4dfdd-114">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="4dfdd-114">Visual Studio for Mac</span></span>](https://www.visualstudio.com/vs/visual-studio-mac/)

## <a name="create-a-web-app"></a><span data-ttu-id="4dfdd-115">Criar um aplicativo Web</span><span class="sxs-lookup"><span data-stu-id="4dfdd-115">Create a web app</span></span>

<span data-ttu-id="4dfdd-116">No Visual Studio, selecione **Arquivo > Nova Solução**.</span><span class="sxs-lookup"><span data-stu-id="4dfdd-116">From Visual Studio, select **File > New Solution**.</span></span>

![Nova solução do macOS](../first-web-api-mac/_static/sln.png)

<span data-ttu-id="4dfdd-118">Selecione **Aplicativo .NET Core > ASP.NET Core > Aplicativo Web > Avançar**.</span><span class="sxs-lookup"><span data-stu-id="4dfdd-118">Select **.NET Core App >  ASP.NET Core > Web App > Next**.</span></span>

![Caixa de diálogo Novo projeto do macOS](start-mvc/1.png)

<span data-ttu-id="4dfdd-120">Nomeie o projeto **MvcMovie** e, em seguida, selecione **Criar**.</span><span class="sxs-lookup"><span data-stu-id="4dfdd-120">Name the project **MvcMovie**, and then select **Create**.</span></span>

![Caixa de diálogo Novo projeto do macOS](start-mvc/2.png)

### <a name="launch-the-app"></a><span data-ttu-id="4dfdd-122">Iniciar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="4dfdd-122">Launch the app</span></span>

<span data-ttu-id="4dfdd-123">No Visual Studio, selecione **Executar > Iniciar Sem Depuração** para iniciar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="4dfdd-123">In Visual Studio, select **Run > Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="4dfdd-124">O Visual Studio inicia o [Kestrel](xref:fundamentals/servers/index#kestrel), inicia um navegador e navega para `http://localhost:port`, em que *porta* é um número da porta escolhido aleatoriamente.</span><span class="sxs-lookup"><span data-stu-id="4dfdd-124">Visual Studio starts [Kestrel](xref:fundamentals/servers/index#kestrel), launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

![Navegador com novo projeto](start-mvc/b1.png)

* <span data-ttu-id="4dfdd-126">A barra de endereços mostra `localhost:port#` e não algo como `example.com`.</span><span class="sxs-lookup"><span data-stu-id="4dfdd-126">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="4dfdd-127">Isso ocorre porque `localhost` é o nome do host padrão do computador local.</span><span class="sxs-lookup"><span data-stu-id="4dfdd-127">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="4dfdd-128">Quando o Visual Studio cria um projeto Web, uma porta aleatória é usada para o servidor Web.</span><span class="sxs-lookup"><span data-stu-id="4dfdd-128">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="4dfdd-129">Quando você executar o aplicativo, verá um número da porta diferente.</span><span class="sxs-lookup"><span data-stu-id="4dfdd-129">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="4dfdd-130">Você pode iniciar o aplicativo no modo de depuração ou sem depuração no item de menu **Executar**.</span><span class="sxs-lookup"><span data-stu-id="4dfdd-130">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

<span data-ttu-id="4dfdd-131">O modelo padrão fornece os links **Página Inicial, Sobre** e **Contato**.</span><span class="sxs-lookup"><span data-stu-id="4dfdd-131">The default template gives you **Home, About** and **Contact** links.</span></span> <span data-ttu-id="4dfdd-132">A imagem do navegador acima não mostra esses links.</span><span class="sxs-lookup"><span data-stu-id="4dfdd-132">The browser image above doesn't show these links.</span></span> <span data-ttu-id="4dfdd-133">Dependendo do tamanho do navegador, talvez você precise clicar no ícone de navegação para mostrá-los.</span><span class="sxs-lookup"><span data-stu-id="4dfdd-133">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Navegador com Novo projeto](start-mvc/b2.png)

<span data-ttu-id="4dfdd-135">Na próxima parte deste tutorial, você saberá mais sobre o MVC e começará a escrever um pouco de código.</span><span class="sxs-lookup"><span data-stu-id="4dfdd-135">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="4dfdd-136">Avançar</span><span class="sxs-lookup"><span data-stu-id="4dfdd-136">Next</span></span>](adding-controller.md)  
