---
title: "Introdução ao ASP.NET Core MVC e ao Visual Studio"
author: rick-anderson
description: "Introdução ao ASP.NET Core MVC e ao Visual Studio"
keywords: ASP.NET Core, MVC
ms.author: riande
manager: wpickett
ms.date: 10/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 4b86eb22e367d2305b7995421aec6f37008c5637
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
# <a name="getting-started-with-aspnet-core-mvc-and-visual-studio"></a><span data-ttu-id="a5f96-104">Introdução ao ASP.NET Core MVC e ao Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a5f96-104">Getting started with ASP.NET Core MVC and Visual Studio</span></span>

<span data-ttu-id="a5f96-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a5f96-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[consider RP](../../includes/razor.md)]

<span data-ttu-id="a5f96-106">Há três versões deste tutorial:</span><span class="sxs-lookup"><span data-stu-id="a5f96-106">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="a5f96-107">macOS: [Criar um aplicativo ASP.NET Core MVC com o Visual Studio para Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="a5f96-107">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="a5f96-108">Windows: [Criar um aplicativo ASP.NET Core MVC com o Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="a5f96-108">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="a5f96-109">macOS, Linux e Windows: [Criar um aplicativo ASP.NET Core MVC com o Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="a5f96-109">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

## <a name="install-visual-studio-and-net-core"></a><span data-ttu-id="a5f96-110">Instalar Visual Studio e .NET Core</span><span class="sxs-lookup"><span data-stu-id="a5f96-110">Install Visual Studio and .NET Core</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a5f96-111">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a5f96-111">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a5f96-112">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a5f96-112">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="a5f96-113">Instale o Visual Studio Community 2017.</span><span class="sxs-lookup"><span data-stu-id="a5f96-113">Install Visual Studio Community 2017.</span></span> <span data-ttu-id="a5f96-114">Selecione o download de comunidade.</span><span class="sxs-lookup"><span data-stu-id="a5f96-114">Select the Community download.</span></span> <span data-ttu-id="a5f96-115">Ignore esta etapa se você tiver o Visual Studio 2017 instalado.</span><span class="sxs-lookup"><span data-stu-id="a5f96-115">Skip this step if you have Visual Studio 2017 installed.</span></span>

* [<span data-ttu-id="a5f96-116">Instalador de home page do Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="a5f96-116">Visual Studio 2017 Home page installer</span></span>](https://www.visualstudio.com/)

<span data-ttu-id="a5f96-117">Execute o instalador e selecione as cargas de trabalho a seguir:</span><span class="sxs-lookup"><span data-stu-id="a5f96-117">Run the installer and select the following workloads:</span></span>

* <span data-ttu-id="a5f96-118">**Desenvolvimento na Web e no ASP.NET** (em **Web e Nuvem**)</span><span class="sxs-lookup"><span data-stu-id="a5f96-118">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
* <span data-ttu-id="a5f96-119">**Desenvolvimento de plataforma cruzada do .NET Core** (em **Outros conjuntos de ferramentas**)</span><span class="sxs-lookup"><span data-stu-id="a5f96-119">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>

![**Desenvolvimento na Web e no ASP.NET** (em **Web e Nuvem**)](start-mvc/_static/web_workload.png)

![**Desenvolvimento de plataforma cruzada do .NET Core** (em **Outros conjuntos de ferramentas**)](start-mvc/_static/x_plat_wl.png)

---

## <a name="create-a-web-app"></a><span data-ttu-id="a5f96-122">Criar um aplicativo Web</span><span class="sxs-lookup"><span data-stu-id="a5f96-122">Create a web app</span></span>

<span data-ttu-id="a5f96-123">No Visual Studio, selecione **Arquivo > Novo > Projeto**.</span><span class="sxs-lookup"><span data-stu-id="a5f96-123">From Visual Studio, select  **File > New > Project**.</span></span>

![Arquivo > Novo > Projeto](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="a5f96-125">Complete a caixa de diálogo **Novo Projeto**:</span><span class="sxs-lookup"><span data-stu-id="a5f96-125">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="a5f96-126">No painel esquerdo, toque em **.NET Core**</span><span class="sxs-lookup"><span data-stu-id="a5f96-126">In the left pane, tap **.NET Core**</span></span>
* <span data-ttu-id="a5f96-127">No painel central, toque em **Aplicativo Web ASP.NET Core (.NET Core)**</span><span class="sxs-lookup"><span data-stu-id="a5f96-127">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="a5f96-128">Nomeie o projeto "MvcMovie" (é importante nomear o projeto "MvcMovie" para que, quando você copiar o código, o namespace corresponda).</span><span class="sxs-lookup"><span data-stu-id="a5f96-128">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="a5f96-129">Toque em **OK**</span><span class="sxs-lookup"><span data-stu-id="a5f96-129">Tap **OK**</span></span>

![<span data-ttu-id="a5f96-130">Caixa de diálogo Novo projeto, .NET Core no painel esquerdo, Web do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a5f96-130">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2.png)


# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a5f96-131">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a5f96-131">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="a5f96-132">Complete a caixa de diálogo **Novo aplicativo Web ASP.NET Core (.NET Core) – MvcMovie**:</span><span class="sxs-lookup"><span data-stu-id="a5f96-132">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="a5f96-133">Na caixa de lista suspensa do seletor de versão, selecione **ASP.NET Core 2.-**</span><span class="sxs-lookup"><span data-stu-id="a5f96-133">In the version selector drop-down box select **ASP.NET Core 2.-**</span></span>
* <span data-ttu-id="a5f96-134">Selecione **Aplicativo Web (Modelo-Exibir-Controlador)**</span><span class="sxs-lookup"><span data-stu-id="a5f96-134">Select **Web Application(Model-View-Controller)**</span></span>
* <span data-ttu-id="a5f96-135">Toque em **OK**.</span><span class="sxs-lookup"><span data-stu-id="a5f96-135">Tap **OK**.</span></span>

![<span data-ttu-id="a5f96-136">Caixa de diálogo Novo projeto, .NET Core no painel esquerdo, Web do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a5f96-136">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a5f96-137">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a5f96-137">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="a5f96-138">Complete a caixa de diálogo **Novo aplicativo Web ASP.NET Core (.NET Core) – MvcMovie**:</span><span class="sxs-lookup"><span data-stu-id="a5f96-138">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="a5f96-139">Na caixa de lista suspensa do seletor de versão, toque em **ASP.NET Core 1.1**</span><span class="sxs-lookup"><span data-stu-id="a5f96-139">In the version selector drop-down box tap **ASP.NET Core 1.1**</span></span>
* <span data-ttu-id="a5f96-140">Toque em **Aplicativo Web**</span><span class="sxs-lookup"><span data-stu-id="a5f96-140">Tap **Web Application**</span></span>
* <span data-ttu-id="a5f96-141">Mantenha o padrão **Sem Autenticação**</span><span class="sxs-lookup"><span data-stu-id="a5f96-141">Keep the default **No Authentication**</span></span>
* <span data-ttu-id="a5f96-142">Toque em **OK**.</span><span class="sxs-lookup"><span data-stu-id="a5f96-142">Tap **OK**.</span></span>

![Novo aplicativo Web ASP.NET Core](start-mvc/_static/p3.png)

---

<span data-ttu-id="a5f96-144">O Visual Studio usou um modelo padrão para o projeto MVC que você acabou de criar.</span><span class="sxs-lookup"><span data-stu-id="a5f96-144">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="a5f96-145">Você pode ter um aplicativo funcionando agora mesmo digitando um nome de projeto e selecionando algumas opções.</span><span class="sxs-lookup"><span data-stu-id="a5f96-145">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="a5f96-146">Este é um projeto inicial simples e é um bom lugar para começar,</span><span class="sxs-lookup"><span data-stu-id="a5f96-146">This is a simple starter project, and it's a good place to start,</span></span>

<span data-ttu-id="a5f96-147">Toque em **F5** para executar o aplicativo no modo de depuração ou **Ctrl-F5** para executá-lo no modo de não depuração.</span><span class="sxs-lookup"><span data-stu-id="a5f96-147">Tap **F5** to run the app in debug mode or **Ctrl-F5** in non-debug mode.</span></span>
<!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![aplicativo em execução](start-mvc/_static/1.png)

* <span data-ttu-id="a5f96-149">O Visual Studio inicia o [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) e executa o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="a5f96-149">Visual Studio starts [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="a5f96-150">Observe que a barra de endereços mostra `localhost:port#` e não algo como `example.com`.</span><span class="sxs-lookup"><span data-stu-id="a5f96-150">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="a5f96-151">Isso ocorre porque `localhost` é o nome do host padrão do computador local.</span><span class="sxs-lookup"><span data-stu-id="a5f96-151">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="a5f96-152">Quando o Visual Studio cria um projeto Web, uma porta aleatória é usada para o servidor Web.</span><span class="sxs-lookup"><span data-stu-id="a5f96-152">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="a5f96-153">Na imagem acima, o número da porta é 5000.</span><span class="sxs-lookup"><span data-stu-id="a5f96-153">In the image above, the port number is 5000.</span></span> <span data-ttu-id="a5f96-154">Quando você executar o aplicativo, você verá um número da porta diferente.</span><span class="sxs-lookup"><span data-stu-id="a5f96-154">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="a5f96-155">Iniciar o aplicativo com **Ctrl+F5** (modo de não depuração) permite que você faça alterações de código, salve o arquivo, atualize o navegador e veja as alterações de código.</span><span class="sxs-lookup"><span data-stu-id="a5f96-155">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="a5f96-156">Muitos desenvolvedores preferem usar modo de não depuração para iniciar o aplicativo e exibir alterações rapidamente.</span><span class="sxs-lookup"><span data-stu-id="a5f96-156">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="a5f96-157">Você pode iniciar o aplicativo no modo de não depuração ou de depuração por meio do item de menu **Depurar**:</span><span class="sxs-lookup"><span data-stu-id="a5f96-157">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![Menu Depurar](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="a5f96-159">Você pode depurar o aplicativo tocando no botão **IIS Express**</span><span class="sxs-lookup"><span data-stu-id="a5f96-159">You can debug the app by tapping the **IIS Express** button</span></span>

![IIS Express](start-mvc/_static/iis_express.png)

<span data-ttu-id="a5f96-161">O modelo padrão fornece os links funcionais **Página Inicial, Sobre** e **Contato**.</span><span class="sxs-lookup"><span data-stu-id="a5f96-161">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="a5f96-162">A imagem do navegador acima não mostra esses links.</span><span class="sxs-lookup"><span data-stu-id="a5f96-162">The browser image above doesn't show these links.</span></span> <span data-ttu-id="a5f96-163">Dependendo do tamanho do navegador, talvez você precise clicar no ícone de navegação para mostrá-los.</span><span class="sxs-lookup"><span data-stu-id="a5f96-163">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![ícone de navegação na parte superior direita](start-mvc/_static/2.png)

<span data-ttu-id="a5f96-165">Se você estava executando no modo de depuração, toque em **Shift-F5** para interromper a depuração.</span><span class="sxs-lookup"><span data-stu-id="a5f96-165">If you were running in debug mode, tap **Shift-F5** to stop debugging.</span></span>

<span data-ttu-id="a5f96-166">Na próxima parte deste tutorial, saberemos mais sobre o MVC e começaremos a escrever um pouco de código.</span><span class="sxs-lookup"><span data-stu-id="a5f96-166">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="a5f96-167">Avançar</span><span class="sxs-lookup"><span data-stu-id="a5f96-167">Next</span></span>](adding-controller.md)  
