---
title: "Introdução ao ASP.NET Core MVC no Mac, Linux ou Windows"
author: rick-anderson
description: "Introdução ao ASP.NET Core MVC e ao Visual Studio Code no Mac, Linux e Windows"
manager: wpickett
ms.author: riande
ms.date: 07/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app-xplat/start-mvc
ms.openlocfilehash: 4771555b66f328a819f17a32eb3959f9ecf33d44
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2018
---
# <a name="getting-started-with-aspnet-core-mvc--on-mac-linux-or-windows"></a><span data-ttu-id="ce66f-103">Introdução ao ASP.NET Core MVC no Mac, Linux ou Windows</span><span class="sxs-lookup"><span data-stu-id="ce66f-103">Getting started with ASP.NET Core MVC  on Mac, Linux, or Windows</span></span>

<span data-ttu-id="ce66f-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ce66f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="ce66f-105">Este tutorial ensinará os conceitos básicos da criação de um aplicativo Web ASP.NET Core MVC usando o [VS Code](https://code.visualstudio.com) (Visual Studio Code).</span><span class="sxs-lookup"><span data-stu-id="ce66f-105">This tutorial will teach you the basics of building an ASP.NET Core MVC web app using [Visual Studio Code](https://code.visualstudio.com) (VS Code).</span></span> <span data-ttu-id="ce66f-106">O tutorial pressupõe que você já tenha familiaridade com o VS Code.</span><span class="sxs-lookup"><span data-stu-id="ce66f-106">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="ce66f-107">Consulte [Introdução ao VS Code](https://code.visualstudio.com/docs) e [Ajuda do Visual Studio Code](#visual-studio-code-help) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="ce66f-107">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span> 

[!INCLUDE[consider RP](../../includes/razor.md)]

<span data-ttu-id="ce66f-108">Há três versões deste tutorial:</span><span class="sxs-lookup"><span data-stu-id="ce66f-108">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="ce66f-109">macOS: [Criar um aplicativo ASP.NET Core MVC com o Visual Studio para Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="ce66f-109">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="ce66f-110">Windows: [Criar um aplicativo ASP.NET Core MVC com o Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="ce66f-110">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="ce66f-111">macOS, Linux e Windows: [Criar um aplicativo ASP.NET Core MVC com o Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="ce66f-111">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span> 

## <a name="install-vs-code-and-net-core"></a><span data-ttu-id="ce66f-112">Instalar o VS Code e o .NET Core</span><span class="sxs-lookup"><span data-stu-id="ce66f-112">Install VS Code and .NET Core</span></span>

<span data-ttu-id="ce66f-113">Este tutorial exige o [SDK do .NET Core 2.0.0](https://www.microsoft.com/net/core) ou posterior.</span><span class="sxs-lookup"><span data-stu-id="ce66f-113">This tutorial requires the [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span> <span data-ttu-id="ce66f-114">Consulte [este pdf](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-mvc-app-mac/start-mvc/8-23-17.pdf) para a versão ASP.NET Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="ce66f-114">See [the pdf](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-mvc-app-mac/start-mvc/8-23-17.pdf) for the ASP.NET Core 1.1 version.</span></span>

<span data-ttu-id="ce66f-115">Instale o seguinte:</span><span class="sxs-lookup"><span data-stu-id="ce66f-115">Install the following:</span></span>

* <span data-ttu-id="ce66f-116">[SDK do .NET Core 2.0.0](https://www.microsoft.com/net/core) ou posterior.</span><span class="sxs-lookup"><span data-stu-id="ce66f-116">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span>
* [<span data-ttu-id="ce66f-117">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ce66f-117">Visual Studio Code</span></span>](https://code.visualstudio.com)
* <span data-ttu-id="ce66f-118">[Extensão C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) do VS Code</span><span class="sxs-lookup"><span data-stu-id="ce66f-118">VS Code [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span></span> 

## <a name="create-a-web-app-with-dotnet"></a><span data-ttu-id="ce66f-119">Criar um aplicativo Web com o dotnet</span><span class="sxs-lookup"><span data-stu-id="ce66f-119">Create a web app with dotnet</span></span>

<span data-ttu-id="ce66f-120">Em um terminal, execute os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="ce66f-120">From a terminal, run the following commands:</span></span>

```console
mkdir MvcMovie
cd MvcMovie
dotnet new mvc
```

<span data-ttu-id="ce66f-121">Abra a pasta *MvcMovie* no Visual Studio Code (VS Code) e selecione o arquivo *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="ce66f-121">Open the *MvcMovie* folder in Visual Studio Code (VS Code) and select the *Startup.cs* file.</span></span>

- <span data-ttu-id="ce66f-122">Selecione **Sim** para a mensagem de **Aviso** – Os ativos necessários para compilar e depurar estão ausentes em “MvcMovie”.</span><span class="sxs-lookup"><span data-stu-id="ce66f-122">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'MvcMovie'.</span></span> <span data-ttu-id="ce66f-123">Deseja adicioná-los?”</span><span class="sxs-lookup"><span data-stu-id="ce66f-123">Add them?"</span></span>
- <span data-ttu-id="ce66f-124">Selecione **Restaurar** para a mensagem **Informações** “Há dependências não resolvidas”.</span><span class="sxs-lookup"><span data-stu-id="ce66f-124">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

![VS Code com o Aviso – Os ativos necessários para compilar e depurar estão ausentes em “MvcMovie”.](../web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="ce66f-128">Pressione **Depurar** (F5) para compilar e executar o programa.</span><span class="sxs-lookup"><span data-stu-id="ce66f-128">Press **Debug** (F5) to build and run the program.</span></span>

![aplicativo em execução](../first-mvc-app/start-mvc/_static/1.png)

<span data-ttu-id="ce66f-130">O VS Code inicia o servidor Web [Kestrel](xref:fundamentals/servers/kestrel) e executa o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="ce66f-130">VS Code starts the [Kestrel](xref:fundamentals/servers/kestrel) web server and runs your app.</span></span> <span data-ttu-id="ce66f-131">Observe que a barra de endereços mostra `localhost:5000` e não algo como `example.com`.</span><span class="sxs-lookup"><span data-stu-id="ce66f-131">Notice that the address bar shows `localhost:5000` and not something like `example.com`.</span></span> <span data-ttu-id="ce66f-132">Isso ocorre porque `localhost` é o nome do host padrão do computador local.</span><span class="sxs-lookup"><span data-stu-id="ce66f-132">That's because `localhost` is the standard hostname for your local computer.</span></span>

<span data-ttu-id="ce66f-133">O modelo padrão fornece os links funcionais **Página Inicial, Sobre** e **Contato**.</span><span class="sxs-lookup"><span data-stu-id="ce66f-133">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="ce66f-134">A imagem do navegador acima não mostra esses links.</span><span class="sxs-lookup"><span data-stu-id="ce66f-134">The browser image above doesn't show these links.</span></span> <span data-ttu-id="ce66f-135">Dependendo do tamanho do navegador, talvez você precise clicar no ícone de navegação para mostrá-los.</span><span class="sxs-lookup"><span data-stu-id="ce66f-135">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![ícone de navegação na parte superior direita](../first-mvc-app/start-mvc/_static/2.png)

<span data-ttu-id="ce66f-137">Na próxima parte deste tutorial, saberemos mais sobre o MVC e começaremos a escrever um pouco de código.</span><span class="sxs-lookup"><span data-stu-id="ce66f-137">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

## <a name="visual-studio-code-help"></a><span data-ttu-id="ce66f-138">Ajuda do Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ce66f-138">Visual Studio Code help</span></span>

- [<span data-ttu-id="ce66f-139">Introdução</span><span class="sxs-lookup"><span data-stu-id="ce66f-139">Getting started</span></span>](https://code.visualstudio.com/docs)
- [<span data-ttu-id="ce66f-140">Depuração</span><span class="sxs-lookup"><span data-stu-id="ce66f-140">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
- [<span data-ttu-id="ce66f-141">Terminal integrado</span><span class="sxs-lookup"><span data-stu-id="ce66f-141">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [<span data-ttu-id="ce66f-142">Atalhos de teclado</span><span class="sxs-lookup"><span data-stu-id="ce66f-142">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [<span data-ttu-id="ce66f-143">Atalhos de teclado do Mac</span><span class="sxs-lookup"><span data-stu-id="ce66f-143">Mac keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [<span data-ttu-id="ce66f-144">Atalhos de teclado do Linux</span><span class="sxs-lookup"><span data-stu-id="ce66f-144">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [<span data-ttu-id="ce66f-145">Atalhos de teclado do Windows</span><span class="sxs-lookup"><span data-stu-id="ce66f-145">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

>[!div class="step-by-step"]
[<span data-ttu-id="ce66f-146">Próximo – Adicionar um controlador</span><span class="sxs-lookup"><span data-stu-id="ce66f-146">Next - Add a controller</span></span>](adding-controller.md)
