---
title: Introdução ao ASP.NET Core MVC e ao Visual Studio
author: rick-anderson
description: Saiba como começar a usar o ASP.NET Core MVC e o Visual Studio.
ms.author: riande
ms.date: 10/07/2017
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 41f986a06ec46dc025c4e8218745b4a513e8ee2a
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011689"
---
# <a name="get-started-with-aspnet-core-mvc-and-visual-studio"></a><span data-ttu-id="7849e-103">Introdução ao ASP.NET Core MVC e ao Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7849e-103">Get started with ASP.NET Core MVC and Visual Studio</span></span>

<span data-ttu-id="7849e-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7849e-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="7849e-105">Há três versões deste tutorial:</span><span class="sxs-lookup"><span data-stu-id="7849e-105">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="7849e-106">macOS: [Criar um aplicativo ASP.NET Core MVC com o Visual Studio para Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="7849e-106">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="7849e-107">Windows: [Como criar um aplicativo ASP.NET Core MVC com o Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="7849e-107">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="7849e-108">macOS, Linux e Windows: [Como criar um aplicativo ASP.NET Core MVC com o Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="7849e-108">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

## <a name="install-visual-studio-and-net-core"></a><span data-ttu-id="7849e-109">Como instalar o Visual Studio e o .NET Core</span><span class="sxs-lookup"><span data-stu-id="7849e-109">Install Visual Studio and .NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-web-app"></a><span data-ttu-id="7849e-110">Como criar um aplicativo Web</span><span class="sxs-lookup"><span data-stu-id="7849e-110">Create a web app</span></span>

<span data-ttu-id="7849e-111">No Visual Studio, selecione **Arquivo > Novo > Projeto**.</span><span class="sxs-lookup"><span data-stu-id="7849e-111">From Visual Studio, select  **File > New > Project**.</span></span>

![Arquivo > Novo > Projeto](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="7849e-113">Complete a caixa de diálogo **Novo Projeto**:</span><span class="sxs-lookup"><span data-stu-id="7849e-113">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="7849e-114">No painel esquerdo, toque em **.NET Core**</span><span class="sxs-lookup"><span data-stu-id="7849e-114">In the left pane, tap **.NET Core**</span></span>
* <span data-ttu-id="7849e-115">No painel central, toque em **Aplicativo Web ASP.NET Core (.NET Core)**</span><span class="sxs-lookup"><span data-stu-id="7849e-115">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="7849e-116">Nomeie o projeto "MvcMovie" (é importante nomear o projeto "MvcMovie" para que, quando você copiar o código, o namespace corresponda).</span><span class="sxs-lookup"><span data-stu-id="7849e-116">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="7849e-117">Toque em **OK**</span><span class="sxs-lookup"><span data-stu-id="7849e-117">Tap **OK**</span></span>

![<span data-ttu-id="7849e-118">Caixa de diálogo Novo projeto, .NET Core no painel esquerdo, Web do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7849e-118">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2-21.png)

<span data-ttu-id="7849e-119">Faça as configurações necessárias na caixa de diálogo **Novo aplicativo Web ASP.NET Core (.NET Core) – MvcMovie**:</span><span class="sxs-lookup"><span data-stu-id="7849e-119">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="7849e-120">Na caixa suspensa do seletor de versão, selecione **ASP.NET Core 2.1**</span><span class="sxs-lookup"><span data-stu-id="7849e-120">In the version selector drop-down box select **ASP.NET Core 2.1**</span></span>
* <span data-ttu-id="7849e-121">Selecione **Aplicativo Web (Modelo-Exibir-Controlador)**</span><span class="sxs-lookup"><span data-stu-id="7849e-121">Select **Web Application(Model-View-Controller)**</span></span>
* <span data-ttu-id="7849e-122">Toque em **OK**.</span><span class="sxs-lookup"><span data-stu-id="7849e-122">Tap **OK**.</span></span>

![<span data-ttu-id="7849e-123">Caixa de diálogo Novo projeto, .NET Core no painel esquerdo, Web do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7849e-123">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="7849e-124">O Visual Studio usou um modelo padrão para o projeto MVC que você acabou de criar.</span><span class="sxs-lookup"><span data-stu-id="7849e-124">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="7849e-125">Para que o aplicativo comece a funcionar agora mesmo, digite um nome de projeto e selecione algumas opções.</span><span class="sxs-lookup"><span data-stu-id="7849e-125">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="7849e-126">Este é um projeto inicial básico e é um bom ponto de partida,</span><span class="sxs-lookup"><span data-stu-id="7849e-126">This is a basic starter project, and it's a good place to start,</span></span>

<span data-ttu-id="7849e-127">Toque em **F5** para executar o aplicativo no modo de depuração ou **Ctrl-F5** para executá-lo no modo de não depuração.</span><span class="sxs-lookup"><span data-stu-id="7849e-127">Tap **F5** to run the app in debug mode or **Ctrl-F5** in non-debug mode.</span></span>
<span data-ttu-id="7849e-128"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![aplicativo em execução](start-mvc/_static/1.png)</span><span class="sxs-lookup"><span data-stu-id="7849e-128"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![running app](start-mvc/_static/1.png)</span></span>

* <span data-ttu-id="7849e-129">O Visual Studio inicia o [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) e executa o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="7849e-129">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="7849e-130">Observe que a barra de endereços mostra `localhost:port#` e não algo como `example.com`.</span><span class="sxs-lookup"><span data-stu-id="7849e-130">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="7849e-131">Isso ocorre porque `localhost` é o nome do host padrão do computador local.</span><span class="sxs-lookup"><span data-stu-id="7849e-131">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="7849e-132">Quando o Visual Studio cria um projeto Web, uma porta aleatória é usada para o servidor Web.</span><span class="sxs-lookup"><span data-stu-id="7849e-132">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="7849e-133">Na imagem acima, o número da porta é 5000.</span><span class="sxs-lookup"><span data-stu-id="7849e-133">In the image above, the port number is 5000.</span></span> <span data-ttu-id="7849e-134">A URL no navegador mostra `localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="7849e-134">The URL in the browser shows `localhost:5000`.</span></span> <span data-ttu-id="7849e-135">Quando você executar o aplicativo, verá um número de porta diferente.</span><span class="sxs-lookup"><span data-stu-id="7849e-135">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="7849e-136">Iniciar o aplicativo com **Ctrl+F5** (modo de não depuração) permite que você faça alterações de código, salve o arquivo, atualize o navegador e veja as alterações de código.</span><span class="sxs-lookup"><span data-stu-id="7849e-136">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="7849e-137">Muitos desenvolvedores preferem usar modo de não depuração para iniciar o aplicativo e exibir alterações rapidamente.</span><span class="sxs-lookup"><span data-stu-id="7849e-137">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="7849e-138">Você pode iniciar o aplicativo no modo de não depuração ou de depuração por meio do item de menu **Depurar**:</span><span class="sxs-lookup"><span data-stu-id="7849e-138">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![Menu Depurar](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="7849e-140">Você pode depurar o aplicativo tocando no botão **IIS Express**</span><span class="sxs-lookup"><span data-stu-id="7849e-140">You can debug the app by tapping the **IIS Express** button</span></span>

![IIS Express](start-mvc/_static/iis_express.png)

<span data-ttu-id="7849e-142">O modelo padrão fornece os links funcionais **Página Inicial, Sobre** e **Contato**.</span><span class="sxs-lookup"><span data-stu-id="7849e-142">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="7849e-143">A imagem do navegador acima não mostra esses links.</span><span class="sxs-lookup"><span data-stu-id="7849e-143">The browser image above doesn't show these links.</span></span> <span data-ttu-id="7849e-144">Dependendo do tamanho do navegador, talvez você precise clicar no ícone de navegação para mostrá-los.</span><span class="sxs-lookup"><span data-stu-id="7849e-144">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![ícone de navegação na parte superior direita](start-mvc/_static/2.png)

<span data-ttu-id="7849e-146">Se você estava executando no modo de depuração, toque em **Shift-F5** para interromper a depuração.</span><span class="sxs-lookup"><span data-stu-id="7849e-146">If you were running in debug mode, tap **Shift-F5** to stop debugging.</span></span>

<span data-ttu-id="7849e-147">Na próxima parte deste tutorial, saberemos mais sobre o MVC e começaremos a escrever um pouco de código.</span><span class="sxs-lookup"><span data-stu-id="7849e-147">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7849e-148">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7849e-148">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!INCLUDE [](~/includes/net-core-prereqs.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7849e-149">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7849e-149">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="7849e-150">Instale o Visual Studio Community 2017.</span><span class="sxs-lookup"><span data-stu-id="7849e-150">Install Visual Studio Community 2017.</span></span> <span data-ttu-id="7849e-151">Selecione o download de comunidade.</span><span class="sxs-lookup"><span data-stu-id="7849e-151">Select the Community download.</span></span> <span data-ttu-id="7849e-152">Ignore esta etapa se você tiver o Visual Studio 2017 instalado.</span><span class="sxs-lookup"><span data-stu-id="7849e-152">Skip this step if you have Visual Studio 2017 installed.</span></span>

* [<span data-ttu-id="7849e-153">Instalador de home page do Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="7849e-153">Visual Studio 2017 Home page installer</span></span>](https://www.visualstudio.com/)

<span data-ttu-id="7849e-154">Execute o instalador e selecione as cargas de trabalho a seguir:</span><span class="sxs-lookup"><span data-stu-id="7849e-154">Run the installer and select the following workloads:</span></span>

* <span data-ttu-id="7849e-155">**Desenvolvimento na Web e no ASP.NET** (em **Web e Nuvem**)</span><span class="sxs-lookup"><span data-stu-id="7849e-155">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
* <span data-ttu-id="7849e-156">**Desenvolvimento de plataforma cruzada do .NET Core** (em **Outros conjuntos de ferramentas**)</span><span class="sxs-lookup"><span data-stu-id="7849e-156">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>

![**Desenvolvimento na Web e no ASP.NET** (em **Web e Nuvem**)](start-mvc/_static/web_workload.png)

![**Desenvolvimento de plataforma cruzada do .NET Core** (em **Outros conjuntos de ferramentas**)](start-mvc/_static/x_plat_wl.png)

---

## <a name="create-a-web-app"></a><span data-ttu-id="7849e-159">Como criar um aplicativo Web</span><span class="sxs-lookup"><span data-stu-id="7849e-159">Create a web app</span></span>

<span data-ttu-id="7849e-160">No Visual Studio, selecione **Arquivo > Novo > Projeto**.</span><span class="sxs-lookup"><span data-stu-id="7849e-160">From Visual Studio, select  **File > New > Project**.</span></span>

![Arquivo > Novo > Projeto](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="7849e-162">Complete a caixa de diálogo **Novo Projeto**:</span><span class="sxs-lookup"><span data-stu-id="7849e-162">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="7849e-163">No painel esquerdo, toque em **.NET Core**</span><span class="sxs-lookup"><span data-stu-id="7849e-163">In the left pane, tap **.NET Core**</span></span>
* <span data-ttu-id="7849e-164">No painel central, toque em **Aplicativo Web ASP.NET Core (.NET Core)**</span><span class="sxs-lookup"><span data-stu-id="7849e-164">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="7849e-165">Nomeie o projeto "MvcMovie" (é importante nomear o projeto "MvcMovie" para que, quando você copiar o código, o namespace corresponda).</span><span class="sxs-lookup"><span data-stu-id="7849e-165">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="7849e-166">Toque em **OK**</span><span class="sxs-lookup"><span data-stu-id="7849e-166">Tap **OK**</span></span>

![<span data-ttu-id="7849e-167">Caixa de diálogo Novo projeto, .NET Core no painel esquerdo, Web do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7849e-167">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7849e-168">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7849e-168">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="7849e-169">Faça as configurações necessárias na caixa de diálogo **Novo aplicativo Web ASP.NET Core (.NET Core) – MvcMovie**:</span><span class="sxs-lookup"><span data-stu-id="7849e-169">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="7849e-170">Na caixa de lista suspensa do seletor de versão, selecione **ASP.NET Core 2.-**</span><span class="sxs-lookup"><span data-stu-id="7849e-170">In the version selector drop-down box select **ASP.NET Core 2.-**</span></span>
* <span data-ttu-id="7849e-171">Selecione **Aplicativo Web (Modelo-Exibir-Controlador)**</span><span class="sxs-lookup"><span data-stu-id="7849e-171">Select **Web Application(Model-View-Controller)**</span></span>
* <span data-ttu-id="7849e-172">Toque em **OK**.</span><span class="sxs-lookup"><span data-stu-id="7849e-172">Tap **OK**.</span></span>

![<span data-ttu-id="7849e-173">Caixa de diálogo Novo projeto, .NET Core no painel esquerdo, Web do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7849e-173">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7849e-174">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7849e-174">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="7849e-175">Complete a caixa de diálogo **Novo aplicativo Web ASP.NET Core (.NET Core) – MvcMovie**:</span><span class="sxs-lookup"><span data-stu-id="7849e-175">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="7849e-176">Na caixa de lista suspensa do seletor de versão, toque em **ASP.NET Core 1.1**</span><span class="sxs-lookup"><span data-stu-id="7849e-176">In the version selector drop-down box tap **ASP.NET Core 1.1**</span></span>
* <span data-ttu-id="7849e-177">Toque em **Aplicativo Web**</span><span class="sxs-lookup"><span data-stu-id="7849e-177">Tap **Web Application**</span></span>
* <span data-ttu-id="7849e-178">Mantenha o padrão **Sem Autenticação**</span><span class="sxs-lookup"><span data-stu-id="7849e-178">Keep the default **No Authentication**</span></span>
* <span data-ttu-id="7849e-179">Toque em **OK**.</span><span class="sxs-lookup"><span data-stu-id="7849e-179">Tap **OK**.</span></span>

![Novo aplicativo Web ASP.NET Core](start-mvc/_static/p3.png)

---

<span data-ttu-id="7849e-181">O Visual Studio usou um modelo padrão para o projeto MVC que você acabou de criar.</span><span class="sxs-lookup"><span data-stu-id="7849e-181">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="7849e-182">Para que o aplicativo comece a funcionar agora mesmo, digite um nome de projeto e selecione algumas opções.</span><span class="sxs-lookup"><span data-stu-id="7849e-182">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="7849e-183">Este é um projeto inicial básico e é um bom ponto de partida,</span><span class="sxs-lookup"><span data-stu-id="7849e-183">This is a basic starter project, and it's a good place to start,</span></span>

<span data-ttu-id="7849e-184">Toque em **F5** para executar o aplicativo no modo de depuração ou **Ctrl-F5** para executá-lo no modo de não depuração.</span><span class="sxs-lookup"><span data-stu-id="7849e-184">Tap **F5** to run the app in debug mode or **Ctrl-F5** in non-debug mode.</span></span>
<span data-ttu-id="7849e-185"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![aplicativo em execução](start-mvc/_static/1.png)</span><span class="sxs-lookup"><span data-stu-id="7849e-185"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![running app](start-mvc/_static/1.png)</span></span>

* <span data-ttu-id="7849e-186">O Visual Studio inicia o [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) e executa o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="7849e-186">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="7849e-187">Observe que a barra de endereços mostra `localhost:port#` e não algo como `example.com`.</span><span class="sxs-lookup"><span data-stu-id="7849e-187">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="7849e-188">Isso ocorre porque `localhost` é o nome do host padrão do computador local.</span><span class="sxs-lookup"><span data-stu-id="7849e-188">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="7849e-189">Quando o Visual Studio cria um projeto Web, uma porta aleatória é usada para o servidor Web.</span><span class="sxs-lookup"><span data-stu-id="7849e-189">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="7849e-190">Na imagem acima, o número da porta é 5000.</span><span class="sxs-lookup"><span data-stu-id="7849e-190">In the image above, the port number is 5000.</span></span> <span data-ttu-id="7849e-191">A URL no navegador mostra `localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="7849e-191">The URL in the browser shows `localhost:5000`.</span></span> <span data-ttu-id="7849e-192">Quando você executar o aplicativo, verá um número de porta diferente.</span><span class="sxs-lookup"><span data-stu-id="7849e-192">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="7849e-193">Iniciar o aplicativo com **Ctrl+F5** (modo de não depuração) permite que você faça alterações de código, salve o arquivo, atualize o navegador e veja as alterações de código.</span><span class="sxs-lookup"><span data-stu-id="7849e-193">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="7849e-194">Muitos desenvolvedores preferem usar modo de não depuração para iniciar o aplicativo e exibir alterações rapidamente.</span><span class="sxs-lookup"><span data-stu-id="7849e-194">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="7849e-195">Você pode iniciar o aplicativo no modo de não depuração ou de depuração por meio do item de menu **Depurar**:</span><span class="sxs-lookup"><span data-stu-id="7849e-195">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![Menu Depurar](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="7849e-197">Você pode depurar o aplicativo tocando no botão **IIS Express**</span><span class="sxs-lookup"><span data-stu-id="7849e-197">You can debug the app by tapping the **IIS Express** button</span></span>

![IIS Express](start-mvc/_static/iis_express.png)

<span data-ttu-id="7849e-199">O modelo padrão fornece os links funcionais **Página Inicial, Sobre** e **Contato**.</span><span class="sxs-lookup"><span data-stu-id="7849e-199">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="7849e-200">A imagem do navegador acima não mostra esses links.</span><span class="sxs-lookup"><span data-stu-id="7849e-200">The browser image above doesn't show these links.</span></span> <span data-ttu-id="7849e-201">Dependendo do tamanho do navegador, talvez você precise clicar no ícone de navegação para mostrá-los.</span><span class="sxs-lookup"><span data-stu-id="7849e-201">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![ícone de navegação na parte superior direita](start-mvc/_static/2.png)

<span data-ttu-id="7849e-203">Se você estava executando no modo de depuração, toque em **Shift-F5** para interromper a depuração.</span><span class="sxs-lookup"><span data-stu-id="7849e-203">If you were running in debug mode, tap **Shift-F5** to stop debugging.</span></span>

<span data-ttu-id="7849e-204">Na próxima parte deste tutorial, saberemos mais sobre o MVC e começaremos a escrever um pouco de código.</span><span class="sxs-lookup"><span data-stu-id="7849e-204">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

::: moniker-end

> [!div class="step-by-step"]
> [<span data-ttu-id="7849e-205">Avançar</span><span class="sxs-lookup"><span data-stu-id="7849e-205">Next</span></span>](adding-controller.md)  
