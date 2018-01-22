---
title: "Introdução ao ASP.NET Core MVC no Mac, Linux ou Windows"
author: rick-anderson
description: "Introdução ao ASP.NET Core MVC e ao Visual Studio Code no Mac, Linux e Windows"
ms.author: riande
manager: wpickett
ms.date: 07/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-xplat/start-mvc
ms.openlocfilehash: f439b6414d95f6edd1c2201c8aee043f1eab9b76
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/19/2018
---
# <a name="getting-started-with-aspnet-core-mvc--on-mac-linux-or-windows"></a><span data-ttu-id="6180b-103">Introdução ao ASP.NET Core MVC no Mac, Linux ou Windows</span><span class="sxs-lookup"><span data-stu-id="6180b-103">Getting started with ASP.NET Core MVC  on Mac, Linux, or Windows</span></span>

<span data-ttu-id="6180b-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="6180b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="6180b-105">Este tutorial ensinará os conceitos básicos da criação de um aplicativo Web ASP.NET Core MVC usando o [VS Code](https://code.visualstudio.com) (Visual Studio Code).</span><span class="sxs-lookup"><span data-stu-id="6180b-105">This tutorial will teach you the basics of building an ASP.NET Core MVC web app using [Visual Studio Code](https://code.visualstudio.com) (VS Code).</span></span> <span data-ttu-id="6180b-106">O tutorial pressupõe que você já tenha familiaridade com o VS Code.</span><span class="sxs-lookup"><span data-stu-id="6180b-106">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="6180b-107">Consulte [Introdução ao VS Code](https://code.visualstudio.com/docs) e [Ajuda do Visual Studio Code](#visual-studio-code-help) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="6180b-107">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span> [!INCLUDE[consider RP](../../includes/razor.md)]

<span data-ttu-id="6180b-108">Há três versões deste tutorial:</span><span class="sxs-lookup"><span data-stu-id="6180b-108">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="6180b-109">macOS: [Criar um aplicativo ASP.NET Core MVC com o Visual Studio para Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="6180b-109">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="6180b-110">Windows: [Criar um aplicativo ASP.NET Core MVC com o Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="6180b-110">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="6180b-111">macOS, Linux e Windows: [Criar um aplicativo ASP.NET Core MVC com o Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="6180b-111">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span> 

## <a name="install-vs-code-and-net-core"></a><span data-ttu-id="6180b-112">Instalar o VS Code e o .NET Core</span><span class="sxs-lookup"><span data-stu-id="6180b-112">Install VS Code and .NET Core</span></span>

<span data-ttu-id="6180b-113">Este tutorial exige o [SDK do .NET Core 2.0.0](https://www.microsoft.com/net/core) ou posterior.</span><span class="sxs-lookup"><span data-stu-id="6180b-113">This tutorial requires the [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span> <span data-ttu-id="6180b-114">Consulte [este pdf](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-mvc-app-mac/start-mvc/8-23-17.pdf) para a versão ASP.NET Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="6180b-114">See [the pdf](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-mvc-app-mac/start-mvc/8-23-17.pdf) for the ASP.NET Core 1.1 version.</span></span>

<span data-ttu-id="6180b-115">Instale o seguinte:</span><span class="sxs-lookup"><span data-stu-id="6180b-115">Install the following:</span></span>

* <span data-ttu-id="6180b-116">[SDK do .NET Core 2.0.0](https://www.microsoft.com/net/core) ou posterior.</span><span class="sxs-lookup"><span data-stu-id="6180b-116">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span>
* [<span data-ttu-id="6180b-117">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6180b-117">Visual Studio Code</span></span>](https://code.visualstudio.com)
* <span data-ttu-id="6180b-118">[Extensão C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) do VS Code</span><span class="sxs-lookup"><span data-stu-id="6180b-118">VS Code [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span></span> 

## <a name="create-a-web-app-with-dotnet"></a><span data-ttu-id="6180b-119">Criar um aplicativo Web com o dotnet</span><span class="sxs-lookup"><span data-stu-id="6180b-119">Create a web app with dotnet</span></span>

<span data-ttu-id="6180b-120">Em um terminal, execute os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="6180b-120">From a terminal, run the following commands:</span></span>

```console
mkdir MvcMovie
cd MvcMovie
dotnet new mvc
```

<span data-ttu-id="6180b-121">Abra a pasta *MvcMovie* no Visual Studio Code (VS Code) e selecione o arquivo *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="6180b-121">Open the *MvcMovie* folder in Visual Studio Code (VS Code) and select the *Startup.cs* file.</span></span>

- <span data-ttu-id="6180b-122">Selecione **Sim** para a mensagem de **Aviso** – Os ativos necessários para compilar e depurar estão ausentes em “MvcMovie”.</span><span class="sxs-lookup"><span data-stu-id="6180b-122">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'MvcMovie'.</span></span> <span data-ttu-id="6180b-123">Deseja adicioná-los?”</span><span class="sxs-lookup"><span data-stu-id="6180b-123">Add them?"</span></span>
- <span data-ttu-id="6180b-124">Selecione **Restaurar** para a mensagem **Informações** “Há dependências não resolvidas”.</span><span class="sxs-lookup"><span data-stu-id="6180b-124">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

![VS Code com o Aviso – Os ativos necessários para compilar e depurar estão ausentes em “MvcMovie”.](../web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="6180b-128">Pressione **Depurar** (F5) para compilar e executar o programa.</span><span class="sxs-lookup"><span data-stu-id="6180b-128">Press **Debug** (F5) to build and run the program.</span></span>

![aplicativo em execução](../first-mvc-app/start-mvc/_static/1.png)

<span data-ttu-id="6180b-130">O VS Code inicia o servidor Web [Kestrel](xref:fundamentals/servers/kestrel) e executa o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="6180b-130">VS Code starts the [Kestrel](xref:fundamentals/servers/kestrel) web server and runs your app.</span></span> <span data-ttu-id="6180b-131">Observe que a barra de endereços mostra `localhost:5000` e não algo como `example.com`.</span><span class="sxs-lookup"><span data-stu-id="6180b-131">Notice that the address bar shows `localhost:5000` and not something like `example.com`.</span></span> <span data-ttu-id="6180b-132">Isso ocorre porque `localhost` é o nome do host padrão do computador local.</span><span class="sxs-lookup"><span data-stu-id="6180b-132">That's because `localhost` is the standard hostname for your local computer.</span></span>

<span data-ttu-id="6180b-133">O modelo padrão fornece os links funcionais **Página Inicial, Sobre** e **Contato**.</span><span class="sxs-lookup"><span data-stu-id="6180b-133">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="6180b-134">A imagem do navegador acima não mostra esses links.</span><span class="sxs-lookup"><span data-stu-id="6180b-134">The browser image above doesn't show these links.</span></span> <span data-ttu-id="6180b-135">Dependendo do tamanho do navegador, talvez você precise clicar no ícone de navegação para mostrá-los.</span><span class="sxs-lookup"><span data-stu-id="6180b-135">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![ícone de navegação na parte superior direita](../first-mvc-app/start-mvc/_static/2.png)

<span data-ttu-id="6180b-137">Na próxima parte deste tutorial, saberemos mais sobre o MVC e começaremos a escrever um pouco de código.</span><span class="sxs-lookup"><span data-stu-id="6180b-137">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

## <a name="visual-studio-code-help"></a><span data-ttu-id="6180b-138">Ajuda do Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6180b-138">Visual Studio Code help</span></span>

- [<span data-ttu-id="6180b-139">Introdução</span><span class="sxs-lookup"><span data-stu-id="6180b-139">Getting started</span></span>](https://code.visualstudio.com/docs)
- [<span data-ttu-id="6180b-140">Depuração</span><span class="sxs-lookup"><span data-stu-id="6180b-140">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
- [<span data-ttu-id="6180b-141">Terminal integrado</span><span class="sxs-lookup"><span data-stu-id="6180b-141">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [<span data-ttu-id="6180b-142">Atalhos de teclado</span><span class="sxs-lookup"><span data-stu-id="6180b-142">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [<span data-ttu-id="6180b-143">Atalhos de teclado do Mac</span><span class="sxs-lookup"><span data-stu-id="6180b-143">Mac keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [<span data-ttu-id="6180b-144">Atalhos de teclado do Linux</span><span class="sxs-lookup"><span data-stu-id="6180b-144">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [<span data-ttu-id="6180b-145">Atalhos de teclado do Windows</span><span class="sxs-lookup"><span data-stu-id="6180b-145">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

>[!div class="step-by-step"]
[<span data-ttu-id="6180b-146">Próximo – Adicionar um controlador</span><span class="sxs-lookup"><span data-stu-id="6180b-146">Next - Add a controller</span></span>](adding-controller.md)
