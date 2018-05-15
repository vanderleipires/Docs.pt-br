---
title: Introdução ao ASP.NET Core MVC no Mac, no Linux ou no Windows
author: rick-anderson
description: Saiba como começar a usar o ASP.NET Core MVC e o Visual Studio Code no Windows, no Linux e no macOS
manager: wpickett
ms.author: riande
ms.date: 07/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app-xplat/start-mvc
ms.openlocfilehash: 50fbd54c6b0cc1146271afda7e45a0dab590dd7d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
# <a name="introduction-to-aspnet-core-mvc-on-macos-linux-or-windows"></a><span data-ttu-id="67d4d-103">Introdução ao ASP.NET Core MVC no Mac, no Linux ou no Windows</span><span class="sxs-lookup"><span data-stu-id="67d4d-103">Introduction to ASP.NET Core MVC on macOS, Linux, or Windows</span></span>

<span data-ttu-id="67d4d-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="67d4d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="67d4d-105">Este tutorial ensinará os conceitos básicos da criação de um aplicativo Web ASP.NET Core MVC usando o [VS Code](https://code.visualstudio.com) (Visual Studio Code).</span><span class="sxs-lookup"><span data-stu-id="67d4d-105">This tutorial will teach you the basics of building an ASP.NET Core MVC web app using [Visual Studio Code](https://code.visualstudio.com) (VS Code).</span></span> <span data-ttu-id="67d4d-106">O tutorial pressupõe que você já tenha familiaridade com o VS Code.</span><span class="sxs-lookup"><span data-stu-id="67d4d-106">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="67d4d-107">Consulte [Introdução ao VS Code](https://code.visualstudio.com/docs) e [Ajuda do Visual Studio Code](#visual-studio-code-help) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="67d4d-107">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span> 

[!INCLUDE [consider RP](../../includes/razor.md)]

<span data-ttu-id="67d4d-108">Há três versões deste tutorial:</span><span class="sxs-lookup"><span data-stu-id="67d4d-108">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="67d4d-109">macOS: [Criar um aplicativo ASP.NET Core MVC com o Visual Studio para Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="67d4d-109">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="67d4d-110">Windows: [Como criar um aplicativo ASP.NET Core MVC com o Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="67d4d-110">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="67d4d-111">macOS, Linux e Windows: [Como criar um aplicativo ASP.NET Core MVC com o Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="67d4d-111">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="67d4d-112">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="67d4d-112">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-a-web-app-with-dotnet"></a><span data-ttu-id="67d4d-113">Criar um aplicativo Web com o dotnet</span><span class="sxs-lookup"><span data-stu-id="67d4d-113">Create a web app with dotnet</span></span>

<span data-ttu-id="67d4d-114">Em um terminal, execute os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="67d4d-114">From a terminal, run the following commands:</span></span>

```console
mkdir MvcMovie
cd MvcMovie
dotnet new mvc
```

<span data-ttu-id="67d4d-115">Abra a pasta *MvcMovie* no Visual Studio Code (VS Code) e selecione o arquivo *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="67d4d-115">Open the *MvcMovie* folder in Visual Studio Code (VS Code) and select the *Startup.cs* file.</span></span>

- <span data-ttu-id="67d4d-116">Selecione **Sim** para a mensagem de **Aviso** – Os ativos necessários para compilar e depurar estão ausentes em “MvcMovie”.</span><span class="sxs-lookup"><span data-stu-id="67d4d-116">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'MvcMovie'.</span></span> <span data-ttu-id="67d4d-117">Deseja adicioná-los?”</span><span class="sxs-lookup"><span data-stu-id="67d4d-117">Add them?"</span></span>
- <span data-ttu-id="67d4d-118">Selecione **Restaurar** para a mensagem **Informações** “Há dependências não resolvidas”.</span><span class="sxs-lookup"><span data-stu-id="67d4d-118">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

![VS Code com o Aviso – Os ativos necessários para compilar e depurar estão ausentes em “MvcMovie”.](../web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="67d4d-122">Pressione **Depurar** (F5) para compilar e executar o programa.</span><span class="sxs-lookup"><span data-stu-id="67d4d-122">Press **Debug** (F5) to build and run the program.</span></span>

![aplicativo em execução](../first-mvc-app/start-mvc/_static/1.png)

<span data-ttu-id="67d4d-124">O VS Code inicia o servidor Web [Kestrel](xref:fundamentals/servers/kestrel) e executa o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="67d4d-124">VS Code starts the [Kestrel](xref:fundamentals/servers/kestrel) web server and runs your app.</span></span> <span data-ttu-id="67d4d-125">Observe que a barra de endereços mostra `localhost:5000` e não algo como `example.com`.</span><span class="sxs-lookup"><span data-stu-id="67d4d-125">Notice that the address bar shows `localhost:5000` and not something like `example.com`.</span></span> <span data-ttu-id="67d4d-126">Isso ocorre porque `localhost` é o nome do host padrão do computador local.</span><span class="sxs-lookup"><span data-stu-id="67d4d-126">That's because `localhost` is the standard hostname for your local computer.</span></span>

<span data-ttu-id="67d4d-127">O modelo padrão fornece os links funcionais **Página Inicial, Sobre** e **Contato**.</span><span class="sxs-lookup"><span data-stu-id="67d4d-127">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="67d4d-128">A imagem do navegador acima não mostra esses links.</span><span class="sxs-lookup"><span data-stu-id="67d4d-128">The browser image above doesn't show these links.</span></span> <span data-ttu-id="67d4d-129">Dependendo do tamanho do navegador, talvez você precise clicar no ícone de navegação para mostrá-los.</span><span class="sxs-lookup"><span data-stu-id="67d4d-129">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![ícone de navegação na parte superior direita](../first-mvc-app/start-mvc/_static/2.png)

<span data-ttu-id="67d4d-131">Na próxima parte deste tutorial, saberemos mais sobre o MVC e começaremos a escrever um pouco de código.</span><span class="sxs-lookup"><span data-stu-id="67d4d-131">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

## <a name="visual-studio-code-help"></a><span data-ttu-id="67d4d-132">Ajuda do Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="67d4d-132">Visual Studio Code help</span></span>

- [<span data-ttu-id="67d4d-133">Introdução</span><span class="sxs-lookup"><span data-stu-id="67d4d-133">Getting started</span></span>](https://code.visualstudio.com/docs)
- [<span data-ttu-id="67d4d-134">Depuração</span><span class="sxs-lookup"><span data-stu-id="67d4d-134">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
- [<span data-ttu-id="67d4d-135">Terminal integrado</span><span class="sxs-lookup"><span data-stu-id="67d4d-135">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [<span data-ttu-id="67d4d-136">Atalhos de teclado</span><span class="sxs-lookup"><span data-stu-id="67d4d-136">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [<span data-ttu-id="67d4d-137">Atalhos de teclado do macOS</span><span class="sxs-lookup"><span data-stu-id="67d4d-137">macOS keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [<span data-ttu-id="67d4d-138">Atalhos de teclado do Linux</span><span class="sxs-lookup"><span data-stu-id="67d4d-138">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [<span data-ttu-id="67d4d-139">Atalhos de teclado do Windows</span><span class="sxs-lookup"><span data-stu-id="67d4d-139">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

> [!div class="step-by-step"]
> [<span data-ttu-id="67d4d-140">Próximo – Adicionar um controlador</span><span class="sxs-lookup"><span data-stu-id="67d4d-140">Next - Add a controller</span></span>](adding-controller.md)
