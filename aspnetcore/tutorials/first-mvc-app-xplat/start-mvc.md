---
title: "Introdução ao ASP.NET Core MVC no Mac, Linux ou Windows"
author: rick-anderson
description: "Introdução ao ASP.NET Core MVC e ao Visual Studio Code no Mac, Linux e Windows"
keywords: ASP.NET Core,MVC,VS Code,Visual Studio Code,Mac,Linux,Windows
ms.author: riande
manager: wpickett
ms.date: 07/07/2017
ms.topic: get-started-article
ms.assetid: 1d18b589-1638-4dc6-1638-fb0f41998d78
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-xplat/start-mvc
ms.openlocfilehash: 87ce5dca011a7bc37d34799db159d933c158cba1
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/22/2017
---
# <a name="getting-started-with-aspnet-core-mvc--on-mac-linux-or-windows"></a><span data-ttu-id="ca7c2-104">Introdução ao ASP.NET Core MVC no Mac, Linux ou Windows</span><span class="sxs-lookup"><span data-stu-id="ca7c2-104">Getting started with ASP.NET Core MVC  on Mac, Linux, or Windows</span></span>

<span data-ttu-id="ca7c2-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ca7c2-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="ca7c2-106">Este tutorial ensinará os conceitos básicos da criação de um aplicativo Web ASP.NET Core MVC usando o [VS Code](https://code.visualstudio.com) (Visual Studio Code).</span><span class="sxs-lookup"><span data-stu-id="ca7c2-106">This tutorial will teach you the basics of building an ASP.NET Core MVC web app using [Visual Studio Code](https://code.visualstudio.com) (VS Code).</span></span> <span data-ttu-id="ca7c2-107">O tutorial pressupõe que você já tenha familiaridade com o VS Code.</span><span class="sxs-lookup"><span data-stu-id="ca7c2-107">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="ca7c2-108">Consulte [Introdução ao VS Code](https://code.visualstudio.com/docs) e [Ajuda do Visual Studio Code](#visual-studio-code-help) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="ca7c2-108">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span> [!INCLUDE[consider RP](../../includes/razor.md)]

<span data-ttu-id="ca7c2-109">Há três versões deste tutorial:</span><span class="sxs-lookup"><span data-stu-id="ca7c2-109">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="ca7c2-110">macOS: [Criar um aplicativo ASP.NET Core MVC com o Visual Studio para Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="ca7c2-110">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="ca7c2-111">Windows: [Criar um aplicativo ASP.NET Core MVC com o Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="ca7c2-111">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="ca7c2-112">macOS, Linux e Windows: [Criar um aplicativo ASP.NET Core MVC com o Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="ca7c2-112">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span> 

## <a name="install-vs-code-and-net-core"></a><span data-ttu-id="ca7c2-113">Instalar o VS Code e o .NET Core</span><span class="sxs-lookup"><span data-stu-id="ca7c2-113">Install VS Code and .NET Core</span></span>

<span data-ttu-id="ca7c2-114">Este tutorial exige o [SDK do .NET Core 2.0.0](https://www.microsoft.com/net/core) ou posterior.</span><span class="sxs-lookup"><span data-stu-id="ca7c2-114">This tutorial requires the [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span> <span data-ttu-id="ca7c2-115">Consulte [este pdf](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-mvc-app-mac/start-mvc/8-23-17.pdf) para a versão ASP.NET Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="ca7c2-115">See [the pdf](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-mvc-app-mac/start-mvc/8-23-17.pdf) for the ASP.NET Core 1.1 version.</span></span>

<span data-ttu-id="ca7c2-116">Instale o seguinte:</span><span class="sxs-lookup"><span data-stu-id="ca7c2-116">Install the following:</span></span>

* <span data-ttu-id="ca7c2-117">[SDK do .NET Core 2.0.0](https://www.microsoft.com/net/core) ou posterior.</span><span class="sxs-lookup"><span data-stu-id="ca7c2-117">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span>
* [<span data-ttu-id="ca7c2-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ca7c2-118">Visual Studio Code</span></span>](https://code.visualstudio.com)
* <span data-ttu-id="ca7c2-119">[Extensão C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) do VS Code</span><span class="sxs-lookup"><span data-stu-id="ca7c2-119">VS Code [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span></span> 

## <a name="create-a-web-app-with-dotnet"></a><span data-ttu-id="ca7c2-120">Criar um aplicativo Web com o dotnet</span><span class="sxs-lookup"><span data-stu-id="ca7c2-120">Create a web app with dotnet</span></span>

<span data-ttu-id="ca7c2-121">Em um terminal, execute os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="ca7c2-121">From a terminal, run the following commands:</span></span>

```console
mkdir MvcMovie
cd MvcMovie
dotnet new mvc
```

<span data-ttu-id="ca7c2-122">Abra a pasta *MvcMovie* no Visual Studio Code (VS Code) e selecione o arquivo *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="ca7c2-122">Open the *MvcMovie* folder in Visual Studio Code (VS Code) and select the *Startup.cs* file.</span></span>

- <span data-ttu-id="ca7c2-123">Selecione **Sim** para a mensagem de **Aviso** – Os ativos necessários para compilar e depurar estão ausentes em “MvcMovie”.</span><span class="sxs-lookup"><span data-stu-id="ca7c2-123">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'MvcMovie'.</span></span> <span data-ttu-id="ca7c2-124">Deseja adicioná-los?”</span><span class="sxs-lookup"><span data-stu-id="ca7c2-124">Add them?"</span></span>
- <span data-ttu-id="ca7c2-125">Selecione **Restaurar** para a mensagem **Informações** “Há dependências não resolvidas”.</span><span class="sxs-lookup"><span data-stu-id="ca7c2-125">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

![VS Code com o Aviso – Os ativos necessários para compilar e depurar estão ausentes em “MvcMovie”.](../web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="ca7c2-129">Pressione **Depurar** (F5) para compilar e executar o programa.</span><span class="sxs-lookup"><span data-stu-id="ca7c2-129">Press **Debug** (F5) to build and run the program.</span></span>

![aplicativo em execução](../first-mvc-app/start-mvc/_static/1.png)

<span data-ttu-id="ca7c2-131">O VS Code inicia o servidor Web [Kestrel](xref:fundamentals/servers/kestrel) e executa o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="ca7c2-131">VS Code starts the [Kestrel](xref:fundamentals/servers/kestrel) web server and runs your app.</span></span> <span data-ttu-id="ca7c2-132">Observe que a barra de endereços mostra `localhost:5000` e não algo como `example.com`.</span><span class="sxs-lookup"><span data-stu-id="ca7c2-132">Notice that the address bar shows `localhost:5000` and not something like `example.com`.</span></span> <span data-ttu-id="ca7c2-133">Isso ocorre porque `localhost` é o nome do host padrão do computador local.</span><span class="sxs-lookup"><span data-stu-id="ca7c2-133">That's because `localhost` is the standard hostname for your local computer.</span></span>

<span data-ttu-id="ca7c2-134">O modelo padrão fornece os links funcionais **Página Inicial, Sobre** e **Contato**.</span><span class="sxs-lookup"><span data-stu-id="ca7c2-134">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="ca7c2-135">A imagem do navegador acima não mostra esses links.</span><span class="sxs-lookup"><span data-stu-id="ca7c2-135">The browser image above doesn't show these links.</span></span> <span data-ttu-id="ca7c2-136">Dependendo do tamanho do navegador, talvez você precise clicar no ícone de navegação para mostrá-los.</span><span class="sxs-lookup"><span data-stu-id="ca7c2-136">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![ícone de navegação na parte superior direita](../first-mvc-app/start-mvc/_static/2.png)

<span data-ttu-id="ca7c2-138">Na próxima parte deste tutorial, saberemos mais sobre o MVC e começaremos a escrever um pouco de código.</span><span class="sxs-lookup"><span data-stu-id="ca7c2-138">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

## <a name="visual-studio-code-help"></a><span data-ttu-id="ca7c2-139">Ajuda do Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ca7c2-139">Visual Studio Code help</span></span>

- [<span data-ttu-id="ca7c2-140">Introdução</span><span class="sxs-lookup"><span data-stu-id="ca7c2-140">Getting started</span></span>](https://code.visualstudio.com/docs)
- [<span data-ttu-id="ca7c2-141">Depuração</span><span class="sxs-lookup"><span data-stu-id="ca7c2-141">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
- [<span data-ttu-id="ca7c2-142">Terminal integrado</span><span class="sxs-lookup"><span data-stu-id="ca7c2-142">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [<span data-ttu-id="ca7c2-143">Atalhos de teclado</span><span class="sxs-lookup"><span data-stu-id="ca7c2-143">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [<span data-ttu-id="ca7c2-144">Atalhos de teclado do Mac</span><span class="sxs-lookup"><span data-stu-id="ca7c2-144">Mac keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [<span data-ttu-id="ca7c2-145">Atalhos de teclado do Linux</span><span class="sxs-lookup"><span data-stu-id="ca7c2-145">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [<span data-ttu-id="ca7c2-146">Atalhos de teclado do Windows</span><span class="sxs-lookup"><span data-stu-id="ca7c2-146">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

>[!div class="step-by-step"]
[<span data-ttu-id="ca7c2-147">Próximo – Adicionar um controlador</span><span class="sxs-lookup"><span data-stu-id="ca7c2-147">Next - Add a controller</span></span>](adding-controller.md)
