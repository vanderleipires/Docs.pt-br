---
title: "Introdução ao ASP.NET Core MVC e ao Visual Studio"
author: rick-anderson
description: "Introdução ao ASP.NET Core MVC e ao Visual Studio"
keywords: ASP.NET Core, MVC
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 6e233a0114967405a67b05365e0125620441efb4
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/22/2017
---
# <a name="getting-started-with-aspnet-core-mvc-and-visual-studio"></a><span data-ttu-id="2a3a7-104">Introdução ao ASP.NET Core MVC e ao Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2a3a7-104">Getting started with ASP.NET Core MVC and Visual Studio</span></span>

<span data-ttu-id="2a3a7-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="2a3a7-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="2a3a7-106">Este tutorial lhe ensinará os conceitos básicos da criação de um aplicativo Web ASP.NET Core MVC usando o [Visual Studio 2017](https://www.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="2a3a7-106">This tutorial will teach you the basics of building an ASP.NET Core MVC web app using [Visual Studio 2017](https://www.visualstudio.com/).</span></span> [!INCLUDE[consider RP](../../includes/razor.md)]

<span data-ttu-id="2a3a7-107">Há três versões deste tutorial:</span><span class="sxs-lookup"><span data-stu-id="2a3a7-107">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="2a3a7-108">macOS: [Criar um aplicativo ASP.NET Core MVC com o Visual Studio para Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="2a3a7-108">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="2a3a7-109">Windows: [Criar um aplicativo ASP.NET Core MVC com o Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="2a3a7-109">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="2a3a7-110">macOS, Linux e Windows: [Criar um aplicativo ASP.NET Core MVC com o Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="2a3a7-110">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

<span data-ttu-id="2a3a7-111">Para obter a versão do Visual Studio 2015 deste tutorial, consulte a [Versão do VS 2015 da documentação do ASP.NET Core no formato PDF](https://github.com/aspnet/Docs/blob/master/aspnetcore/common/_static/aspnet-core-project-json.pdf).</span><span class="sxs-lookup"><span data-stu-id="2a3a7-111">For the Visual Studio 2015 version of this tutorial, see the [VS 2015 version of ASP.NET Core documentation in PDF format](https://github.com/aspnet/Docs/blob/master/aspnetcore/common/_static/aspnet-core-project-json.pdf).</span></span>

## <a name="install-visual-studio-and-net-core"></a><span data-ttu-id="2a3a7-112">Instalar Visual Studio e .NET Core</span><span class="sxs-lookup"><span data-stu-id="2a3a7-112">Install Visual Studio and .NET Core</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2a3a7-113">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2a3a7-113">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2a3a7-114">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2a3a7-114">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="2a3a7-115">Instale o Visual Studio Community 2017.</span><span class="sxs-lookup"><span data-stu-id="2a3a7-115">Install Visual Studio Community 2017.</span></span> <span data-ttu-id="2a3a7-116">Selecione o download de comunidade.</span><span class="sxs-lookup"><span data-stu-id="2a3a7-116">Select the Community download.</span></span> <span data-ttu-id="2a3a7-117">Ignore esta etapa se você tiver o Visual Studio 2017 instalado.</span><span class="sxs-lookup"><span data-stu-id="2a3a7-117">Skip this step if you have Visual Studio 2017 installed.</span></span>

* [<span data-ttu-id="2a3a7-118">Instalador de home page do Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="2a3a7-118">Visual Studio 2017 Home page installer</span></span>](https://www.visualstudio.com/)

<span data-ttu-id="2a3a7-119">Execute o instalador e selecione as cargas de trabalho a seguir:</span><span class="sxs-lookup"><span data-stu-id="2a3a7-119">Run the installer and select the following workloads:</span></span>

* <span data-ttu-id="2a3a7-120">**Desenvolvimento na Web e no ASP.NET** (em **Web e Nuvem**)</span><span class="sxs-lookup"><span data-stu-id="2a3a7-120">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
* <span data-ttu-id="2a3a7-121">**Desenvolvimento de plataforma cruzada do .NET Core** (em **Outros conjuntos de ferramentas**)</span><span class="sxs-lookup"><span data-stu-id="2a3a7-121">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>

![**Desenvolvimento na Web e no ASP.NET** (em **Web e Nuvem**)](start-mvc/_static/web_workload.png)

![**Desenvolvimento de plataforma cruzada do .NET Core** (em **Outros conjuntos de ferramentas**)](start-mvc/_static/x_plat_wl.png)

---

## <a name="create-a-web-app"></a><span data-ttu-id="2a3a7-124">Criar um aplicativo Web</span><span class="sxs-lookup"><span data-stu-id="2a3a7-124">Create a web app</span></span>

<span data-ttu-id="2a3a7-125">No Visual Studio, selecione **Arquivo > Novo > Projeto**.</span><span class="sxs-lookup"><span data-stu-id="2a3a7-125">From Visual Studio, select  **File > New > Project**.</span></span>

![Arquivo > Novo > Projeto](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="2a3a7-127">Complete a caixa de diálogo **Novo Projeto**:</span><span class="sxs-lookup"><span data-stu-id="2a3a7-127">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="2a3a7-128">No painel esquerdo, toque em **.NET Core**</span><span class="sxs-lookup"><span data-stu-id="2a3a7-128">In the left pane, tap **.NET Core**</span></span>
* <span data-ttu-id="2a3a7-129">No painel central, toque em **Aplicativo Web ASP.NET Core (.NET Core)**</span><span class="sxs-lookup"><span data-stu-id="2a3a7-129">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="2a3a7-130">Nomeie o projeto "MvcMovie" (é importante nomear o projeto "MvcMovie" para que, quando você copiar o código, o namespace corresponda).</span><span class="sxs-lookup"><span data-stu-id="2a3a7-130">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="2a3a7-131">Toque em **OK**</span><span class="sxs-lookup"><span data-stu-id="2a3a7-131">Tap **OK**</span></span>

![<span data-ttu-id="2a3a7-132">Caixa de diálogo Novo projeto, .NET Core no painel esquerdo, Web do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2a3a7-132">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2.png)


# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2a3a7-133">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2a3a7-133">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="2a3a7-134">Complete a caixa de diálogo **Novo aplicativo Web ASP.NET Core (.NET Core) – MvcMovie**:</span><span class="sxs-lookup"><span data-stu-id="2a3a7-134">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="2a3a7-135">Na caixa de lista suspensa do seletor de versão, selecione **ASP.NET Core 2.-**</span><span class="sxs-lookup"><span data-stu-id="2a3a7-135">In the version selector drop-down box select **ASP.NET Core 2.-**</span></span>
* <span data-ttu-id="2a3a7-136">Selecione **Aplicativo Web (Modelo-Exibir-Controlador)**</span><span class="sxs-lookup"><span data-stu-id="2a3a7-136">Select **Web Application(Model-View-Controller)**</span></span>
* <span data-ttu-id="2a3a7-137">Toque em **OK**.</span><span class="sxs-lookup"><span data-stu-id="2a3a7-137">Tap **OK**.</span></span>

![<span data-ttu-id="2a3a7-138">Caixa de diálogo Novo projeto, .NET Core no painel esquerdo, Web do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2a3a7-138">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2a3a7-139">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2a3a7-139">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="2a3a7-140">Complete a caixa de diálogo **Novo aplicativo Web ASP.NET Core (.NET Core) – MvcMovie**:</span><span class="sxs-lookup"><span data-stu-id="2a3a7-140">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="2a3a7-141">Na caixa de lista suspensa do seletor de versão, toque em **ASP.NET Core 1.1**</span><span class="sxs-lookup"><span data-stu-id="2a3a7-141">In the version selector drop-down box tap **ASP.NET Core 1.1**</span></span>
* <span data-ttu-id="2a3a7-142">Toque em **Aplicativo Web**</span><span class="sxs-lookup"><span data-stu-id="2a3a7-142">Tap **Web Application**</span></span>
* <span data-ttu-id="2a3a7-143">Mantenha o padrão **Sem Autenticação**</span><span class="sxs-lookup"><span data-stu-id="2a3a7-143">Keep the default **No Authentication**</span></span>
* <span data-ttu-id="2a3a7-144">Toque em **OK**.</span><span class="sxs-lookup"><span data-stu-id="2a3a7-144">Tap **OK**.</span></span>

![Novo aplicativo Web ASP.NET Core](start-mvc/_static/p3.png)

---

<span data-ttu-id="2a3a7-146">O Visual Studio usou um modelo padrão para o projeto MVC que você acabou de criar.</span><span class="sxs-lookup"><span data-stu-id="2a3a7-146">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="2a3a7-147">Você pode ter um aplicativo funcionando agora mesmo digitando um nome de projeto e selecionando algumas opções.</span><span class="sxs-lookup"><span data-stu-id="2a3a7-147">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="2a3a7-148">Este é um projeto inicial simples e é um bom lugar para começar,</span><span class="sxs-lookup"><span data-stu-id="2a3a7-148">This is a simple starter project, and it's a good place to start,</span></span>

<span data-ttu-id="2a3a7-149">Toque em **F5** para executar o aplicativo no modo de depuração ou **Ctrl-F5** para executá-lo no modo de não depuração.</span><span class="sxs-lookup"><span data-stu-id="2a3a7-149">Tap **F5** to run the app in debug mode or **Ctrl-F5** in non-debug mode.</span></span>
<!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![aplicativo em execução](start-mvc/_static/1.png)

* <span data-ttu-id="2a3a7-151">O Visual Studio inicia o [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) e executa o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="2a3a7-151">Visual Studio starts [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="2a3a7-152">Observe que a barra de endereços mostra `localhost:port#` e não algo como `example.com`.</span><span class="sxs-lookup"><span data-stu-id="2a3a7-152">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="2a3a7-153">Isso ocorre porque `localhost` é o nome do host padrão do computador local.</span><span class="sxs-lookup"><span data-stu-id="2a3a7-153">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="2a3a7-154">Quando o Visual Studio cria um projeto Web, uma porta aleatória é usada para o servidor Web.</span><span class="sxs-lookup"><span data-stu-id="2a3a7-154">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="2a3a7-155">Na imagem acima, o número da porta é 5000.</span><span class="sxs-lookup"><span data-stu-id="2a3a7-155">In the image above, the port number is 5000.</span></span> <span data-ttu-id="2a3a7-156">Quando você executar o aplicativo, você verá um número da porta diferente.</span><span class="sxs-lookup"><span data-stu-id="2a3a7-156">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="2a3a7-157">Iniciar o aplicativo com **Ctrl+F5** (modo de não depuração) permite que você faça alterações de código, salve o arquivo, atualize o navegador e veja as alterações de código.</span><span class="sxs-lookup"><span data-stu-id="2a3a7-157">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="2a3a7-158">Muitos desenvolvedores preferem usar modo de não depuração para iniciar o aplicativo e exibir alterações rapidamente.</span><span class="sxs-lookup"><span data-stu-id="2a3a7-158">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="2a3a7-159">Você pode iniciar o aplicativo no modo de não depuração ou de depuração por meio do item de menu **Depurar**:</span><span class="sxs-lookup"><span data-stu-id="2a3a7-159">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![Menu Depurar](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="2a3a7-161">Você pode depurar o aplicativo tocando no botão **IIS Express**</span><span class="sxs-lookup"><span data-stu-id="2a3a7-161">You can debug the app by tapping the **IIS Express** button</span></span>

![IIS Express](start-mvc/_static/iis_express.png)

<span data-ttu-id="2a3a7-163">O modelo padrão fornece os links funcionais **Página Inicial, Sobre** e **Contato**.</span><span class="sxs-lookup"><span data-stu-id="2a3a7-163">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="2a3a7-164">A imagem do navegador acima não mostra esses links.</span><span class="sxs-lookup"><span data-stu-id="2a3a7-164">The browser image above doesn't show these links.</span></span> <span data-ttu-id="2a3a7-165">Dependendo do tamanho do navegador, talvez você precise clicar no ícone de navegação para mostrá-los.</span><span class="sxs-lookup"><span data-stu-id="2a3a7-165">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![ícone de navegação na parte superior direita](start-mvc/_static/2.png)

<span data-ttu-id="2a3a7-167">Se você estava executando no modo de depuração, toque em **Shift-F5** para interromper a depuração.</span><span class="sxs-lookup"><span data-stu-id="2a3a7-167">If you were running in debug mode, tap **Shift-F5** to stop debugging.</span></span>

<span data-ttu-id="2a3a7-168">Na próxima parte deste tutorial, saberemos mais sobre o MVC e começaremos a escrever um pouco de código.</span><span class="sxs-lookup"><span data-stu-id="2a3a7-168">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="2a3a7-169">Avançar</span><span class="sxs-lookup"><span data-stu-id="2a3a7-169">Next</span></span>](adding-controller.md)  
