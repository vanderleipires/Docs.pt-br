---
title: "Introdução a Páginas do Razor no ASP.NET Core"
author: rick-anderson
description: "Introdução a Páginas do Razor no ASP.NET Core"
keywords: "ASP.NET Core, Páginas do Razor, Razor, MVC"
ms.author: riande
manager: wpickett
ms.date: 8/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: c22ee2554992d15df2f6b92ee5da6805ab80b73a
ms.sourcegitcommit: d9ec19e5452af83648074db5d96c0a0f4f9e7f9a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/29/2017
---
# <a name="getting-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="16b7d-104">Introdução a Páginas do Razor no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="16b7d-104">Getting started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="16b7d-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="16b7d-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="16b7d-106">Este tutorial ensina as noções básicas de criação de um aplicativo Web de Páginas do Razor do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="16b7d-106">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="16b7d-107">Recomendamos que você conclua a [Introdução a Páginas do Razor](xref:mvc/razor-pages/index) antes de iniciar este tutorial.</span><span class="sxs-lookup"><span data-stu-id="16b7d-107">We recommend you complete [Introduction to Razor Pages](xref:mvc/razor-pages/index) before starting this tutorial.</span></span> <span data-ttu-id="16b7d-108">Páginas do Razor é a maneira recomendada para criar a interface do usuário para aplicativos Web no ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="16b7d-108">Razor Pages is the recommended way to build UI for web applications in ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="16b7d-109">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="16b7d-109">Prerequisites</span></span>

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="16b7d-110">Criar um aplicativo Web do Razor</span><span class="sxs-lookup"><span data-stu-id="16b7d-110">Create a Razor web app</span></span>

* <span data-ttu-id="16b7d-111">No menu **Arquivo** do Visual Studio, selecione **Novo > Projeto**.</span><span class="sxs-lookup"><span data-stu-id="16b7d-111">From the Visual Studio **File** menu, select **New > Project**.</span></span>
* <span data-ttu-id="16b7d-112">Crie um novo Aplicativo Web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="16b7d-112">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="16b7d-113">Nomeie o projeto **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="16b7d-113">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="16b7d-114">É importante nomear o projeto *RazorPagesMovie* de modo que os namespaces façam a correspondência quando você copiar/colar código.</span><span class="sxs-lookup"><span data-stu-id="16b7d-114">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
 <span data-ttu-id="16b7d-115">![novo aplicativo Web ASP.NET Core](../../mvc/razor-pages/index/_static/np.png)</span><span class="sxs-lookup"><span data-stu-id="16b7d-115">![new ASP.NET Core Web Application](../../mvc/razor-pages/index/_static/np.png)</span></span>
* <span data-ttu-id="16b7d-116">Selecione **ASP.NET Core 2.0** na lista suspensa e selecione **Aplicativo Web**.</span><span class="sxs-lookup"><span data-stu-id="16b7d-116">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span>
 <span data-ttu-id="16b7d-117">![Aplicativo Web (Páginas do Razor)](../../mvc/razor-pages/index/_static/np2.png)</span><span class="sxs-lookup"><span data-stu-id="16b7d-117">![Web Application (Razor Pages)](../../mvc/razor-pages/index/_static/np2.png)</span></span>

<span data-ttu-id="16b7d-118">O modelo do Visual Studio cria um projeto inicial:</span><span class="sxs-lookup"><span data-stu-id="16b7d-118">The Visual Studio template creates a starter project:</span></span>

![Gerenciador de Soluções](razor-pages-start/_static/se.png)

<span data-ttu-id="16b7d-120">Pressione **F5** para executar o aplicativo no modo de depuração ou **Ctrl-F5** para executar sem anexar o depurador</span><span class="sxs-lookup"><span data-stu-id="16b7d-120">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger</span></span>

![Página Inicial ou de Índice](razor-pages-start/_static/home.png)

* <span data-ttu-id="16b7d-122">O Visual Studio inicia o [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) e executa o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="16b7d-122">Visual Studio starts [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="16b7d-123">A barra de endereços mostra `localhost:port#` e não algo como `example.com`.</span><span class="sxs-lookup"><span data-stu-id="16b7d-123">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="16b7d-124">Isso ocorre porque `localhost` é o nome do host padrão do computador local.</span><span class="sxs-lookup"><span data-stu-id="16b7d-124">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="16b7d-125">Localhost serve somente solicitações da Web do computador local.</span><span class="sxs-lookup"><span data-stu-id="16b7d-125">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="16b7d-126">Quando o Visual Studio cria um projeto Web, uma porta aleatória é usada para o servidor Web.</span><span class="sxs-lookup"><span data-stu-id="16b7d-126">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="16b7d-127">Na imagem anterior, o número da porta é 5000.</span><span class="sxs-lookup"><span data-stu-id="16b7d-127">In the preceding image, the port number is 5000.</span></span> <span data-ttu-id="16b7d-128">Quando você executar o aplicativo, você verá um número da porta diferente.</span><span class="sxs-lookup"><span data-stu-id="16b7d-128">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="16b7d-129">Iniciar o aplicativo com **Ctrl+F5** (modo de não depuração) permite que você faça alterações de código, salve o arquivo, atualize o navegador e veja as alterações de código.</span><span class="sxs-lookup"><span data-stu-id="16b7d-129">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="16b7d-130">Muitos desenvolvedores preferem usar modo de não depuração para iniciar o aplicativo e exibir alterações rapidamente.</span><span class="sxs-lookup"><span data-stu-id="16b7d-130">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

>[!div class="step-by-step"]
[<span data-ttu-id="16b7d-131">Próximo: adicionando um modelo</span><span class="sxs-lookup"><span data-stu-id="16b7d-131">Next: Adding a model</span></span>](xref:tutorials/razor-pages/modelz)  
