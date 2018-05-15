---
title: Introdução ao ASP.NET Core MVC e ao Visual Studio para Mac
author: rick-anderson
description: Saiba como começar a usar o ASP.NET Core MVC e o Visual Studio
manager: wpickett
ms.author: riande
ms.date: 8/23/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/first-mvc-app-mac/start-mvc
ms.openlocfilehash: ffa620f07251c52c785672d8fbeefacac31ed4c1
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
# <a name="get-started-with-aspnet-core-mvc-and-visual-studio-for-mac"></a><span data-ttu-id="2cfec-103">Introdução ao ASP.NET Core MVC e ao Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="2cfec-103">Get started with ASP.NET Core MVC and Visual Studio for Mac</span></span>

<span data-ttu-id="2cfec-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="2cfec-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="2cfec-105">Este tutorial ensina os conceitos básicos da criação de um aplicativo Web ASP.NET Core MVC usando o [Visual Studio para Mac](https://www.visualstudio.com/vs/visual-studio-mac/).</span><span class="sxs-lookup"><span data-stu-id="2cfec-105">This tutorial teaches you the basics of building an ASP.NET Core MVC web app using [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/).</span></span> 

[!INCLUDE [consider RP](../../includes/razor.md)]

<span data-ttu-id="2cfec-106">Há três versões deste tutorial:</span><span class="sxs-lookup"><span data-stu-id="2cfec-106">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="2cfec-107">macOS: [Compilar um aplicativo ASP.NET Core MVC com o Visual Studio para Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="2cfec-107">macOS: [Build an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="2cfec-108">Windows: [Compilar um aplicativo ASP.NET Core MVC com o Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="2cfec-108">Windows: [Build an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="2cfec-109">Linux, macOS e Windows: [Compilar um aplicativo ASP.NET Core MVC com o Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="2cfec-109">Linux, macOS, and Windows: [Build an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2cfec-110">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="2cfec-110">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-macos.md)]

## <a name="create-a-web-app"></a><span data-ttu-id="2cfec-111">Como criar um aplicativo Web</span><span class="sxs-lookup"><span data-stu-id="2cfec-111">Create a web app</span></span>

<span data-ttu-id="2cfec-112">No Visual Studio, selecione **Arquivo > Nova Solução**.</span><span class="sxs-lookup"><span data-stu-id="2cfec-112">From Visual Studio, select **File > New Solution**.</span></span>

![Nova solução do macOS](../first-web-api-mac/_static/sln.png)

<span data-ttu-id="2cfec-114">Selecione **Aplicativo .NET Core > ASP.NET Core > Aplicativo Web > Avançar**.</span><span class="sxs-lookup"><span data-stu-id="2cfec-114">Select **.NET Core App >  ASP.NET Core > Web App > Next**.</span></span>

![Caixa de diálogo Novo projeto do macOS](start-mvc/1.png)

<span data-ttu-id="2cfec-116">Nomeie o projeto **MvcMovie** e, em seguida, selecione **Criar**.</span><span class="sxs-lookup"><span data-stu-id="2cfec-116">Name the project **MvcMovie**, and then select **Create**.</span></span>

![Caixa de diálogo Novo projeto do macOS](start-mvc/2.png)

### <a name="launch-the-app"></a><span data-ttu-id="2cfec-118">Iniciar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="2cfec-118">Launch the app</span></span>

<span data-ttu-id="2cfec-119">No Visual Studio, selecione **Executar > Iniciar Sem Depuração** para iniciar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="2cfec-119">In Visual Studio, select **Run > Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="2cfec-120">O Visual Studio inicia o [Kestrel](xref:fundamentals/servers/index#kestrel), inicia um navegador e navega para `http://localhost:port`, em que *porta* é um número da porta escolhido aleatoriamente.</span><span class="sxs-lookup"><span data-stu-id="2cfec-120">Visual Studio starts [Kestrel](xref:fundamentals/servers/index#kestrel), launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

![Navegador com novo projeto](start-mvc/b1.png)

* <span data-ttu-id="2cfec-122">A barra de endereços mostra `localhost:port#` e não algo como `example.com`.</span><span class="sxs-lookup"><span data-stu-id="2cfec-122">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="2cfec-123">Isso ocorre porque `localhost` é o nome do host padrão do computador local.</span><span class="sxs-lookup"><span data-stu-id="2cfec-123">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="2cfec-124">Quando o Visual Studio cria um projeto Web, uma porta aleatória é usada para o servidor Web.</span><span class="sxs-lookup"><span data-stu-id="2cfec-124">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="2cfec-125">Quando você executar o aplicativo, verá um número da porta diferente.</span><span class="sxs-lookup"><span data-stu-id="2cfec-125">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="2cfec-126">Você pode iniciar o aplicativo no modo de depuração ou sem depuração no item de menu **Executar**.</span><span class="sxs-lookup"><span data-stu-id="2cfec-126">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

<span data-ttu-id="2cfec-127">O modelo padrão fornece os links **Página Inicial, Sobre** e **Contato**.</span><span class="sxs-lookup"><span data-stu-id="2cfec-127">The default template gives you **Home, About** and **Contact** links.</span></span> <span data-ttu-id="2cfec-128">A imagem do navegador acima não mostra esses links.</span><span class="sxs-lookup"><span data-stu-id="2cfec-128">The browser image above doesn't show these links.</span></span> <span data-ttu-id="2cfec-129">Dependendo do tamanho do navegador, talvez você precise clicar no ícone de navegação para mostrá-los.</span><span class="sxs-lookup"><span data-stu-id="2cfec-129">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Navegador com Novo projeto](start-mvc/b2.png)

<span data-ttu-id="2cfec-131">Na próxima parte deste tutorial, você saberá mais sobre o MVC e começará a escrever um pouco de código.</span><span class="sxs-lookup"><span data-stu-id="2cfec-131">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="2cfec-132">Avançar</span><span class="sxs-lookup"><span data-stu-id="2cfec-132">Next</span></span>](adding-controller.md)  
