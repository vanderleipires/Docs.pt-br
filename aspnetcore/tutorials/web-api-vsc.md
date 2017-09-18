---
title: Criar uma API Web com o ASP.NET Core e o VS Code
author: rick-anderson
description: Compilar uma API Web no macOS, Linux ou Windows com o ASP.NET Core MVC e o Visual Studio Code
keywords: "ASP.NET Core, WebAPI, API Web, REST, Mac, Linux, HTTP, Serviço, Serviço HTTP, VS Code"
ms.author: riande
manager: wpickett
ms.date: 5/24/2017
ms.topic: get-started-article
ms.assetid: 830b4bf5-dd14-423e-9f59-764a6f13a8f6
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/web-api-vsc
ms.openlocfilehash: 17687e38aae066bdab4663268a2af54f20a6ad75
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/12/2017
---
# <a name="create-a-web-api-with-aspnet-core-mvc-and-visual-studio-code-on-linux-macos-and-windows"></a><span data-ttu-id="aa709-104">Criar uma API Web com o ASP.NET Core MVC e o Visual Studio Code no Linux, macOS e Windows</span><span class="sxs-lookup"><span data-stu-id="aa709-104">Create a Web API with ASP.NET Core MVC and Visual Studio Code on Linux, macOS, and Windows</span></span>

<span data-ttu-id="aa709-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT) e [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="aa709-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="aa709-106">Neste tutorial, você compilará uma API Web para gerenciar uma lista de itens de “tarefas pendentes”.</span><span class="sxs-lookup"><span data-stu-id="aa709-106">In this tutorial, you’ll build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="aa709-107">Você não compilará uma interface do usuário.</span><span class="sxs-lookup"><span data-stu-id="aa709-107">You won’t build a UI.</span></span>

<span data-ttu-id="aa709-108">Há três versões deste tutorial:</span><span class="sxs-lookup"><span data-stu-id="aa709-108">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="aa709-109">macOS, Linux, Windows: API Web com o Visual Studio Code (Este tutorial)</span><span class="sxs-lookup"><span data-stu-id="aa709-109">macOS, Linux, Windows: Web API with Visual Studio Code (This tutorial)</span></span>
* <span data-ttu-id="aa709-110">macOS: [API Web com o Visual Studio para Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="aa709-110">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="aa709-111">Windows: [API Web com o Visual Studio para Windows](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="aa709-111">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

## <a name="set-up-your-development-environment"></a><span data-ttu-id="aa709-112">Configurar o ambiente de desenvolvimento</span><span class="sxs-lookup"><span data-stu-id="aa709-112">Set up your development environment</span></span>

<span data-ttu-id="aa709-113">Baixe e instale:</span><span class="sxs-lookup"><span data-stu-id="aa709-113">Download and install:</span></span>
- [<span data-ttu-id="aa709-114">.NET Core</span><span class="sxs-lookup"><span data-stu-id="aa709-114">.NET Core</span></span>](https://www.microsoft.com/net/core)
- [<span data-ttu-id="aa709-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="aa709-115">Visual Studio Code</span></span>](https://code.visualstudio.com)
- <span data-ttu-id="aa709-116">[Extensão do C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) do Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="aa709-116">Visual Studio Code [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span></span>

## <a name="create-the-project"></a><span data-ttu-id="aa709-117">Criar o projeto</span><span class="sxs-lookup"><span data-stu-id="aa709-117">Create the project</span></span>

<span data-ttu-id="aa709-118">Em um console, execute os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="aa709-118">From a console, run the following commands:</span></span>

```console
mkdir TodoApi
cd TodoApi
dotnet new webapi
```

<span data-ttu-id="aa709-119">Abra a pasta *TodoApi* no Visual Studio Code (VS Code) e selecione o arquivo *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="aa709-119">Open the *TodoApi* folder in Visual Studio Code (VS Code) and select the *Startup.cs* file.</span></span>

- <span data-ttu-id="aa709-120">Selecione **Sim** para a mensagem de **Aviso** “Os ativos necessários para compilar e depurar estão ausentes em 'TodoApi'.</span><span class="sxs-lookup"><span data-stu-id="aa709-120">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'TodoApi'.</span></span> <span data-ttu-id="aa709-121">Deseja adicioná-los?”</span><span class="sxs-lookup"><span data-stu-id="aa709-121">Add them?"</span></span>
- <span data-ttu-id="aa709-122">Selecione **Restaurar** para a mensagem **Informações** “Há dependências não resolvidas”.</span><span class="sxs-lookup"><span data-stu-id="aa709-122">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

![VS Code com o Aviso – Os ativos necessários para compilar e depurar estão ausentes em “TodoApi”.](web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="aa709-126">Pressione **Depurar** (F5) para compilar e executar o programa.</span><span class="sxs-lookup"><span data-stu-id="aa709-126">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="aa709-127">Em um navegador, navegue para http://localhost:5000/api/values.</span><span class="sxs-lookup"><span data-stu-id="aa709-127">In a browser navigate to http://localhost:5000/api/values .</span></span> <span data-ttu-id="aa709-128">O seguinte é exibido:</span><span class="sxs-lookup"><span data-stu-id="aa709-128">The following is displayed:</span></span>

`["value1","value2"]`

<span data-ttu-id="aa709-129">Consulte [Ajuda do Visual Studio Code](#visual-studio-code-help) para obter dicas sobre como usar o VS Code.</span><span class="sxs-lookup"><span data-stu-id="aa709-129">See [Visual Studio Code help](#visual-studio-code-help) for tips on using VS Code.</span></span>

## <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="aa709-130">Adicionar suporte ao Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="aa709-130">Add support for Entity Framework Core</span></span>

<span data-ttu-id="aa709-131">Edite o arquivo *TodoApi.csproj* para instalar o provedor de banco de dados [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/).</span><span class="sxs-lookup"><span data-stu-id="aa709-131">Edit the *TodoApi.csproj* file to install the [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) database provider.</span></span> <span data-ttu-id="aa709-132">Este provedor de banco de dados permite que o Entity Framework Core seja usado com um banco de dados em memória.</span><span class="sxs-lookup"><span data-stu-id="aa709-132">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

<span data-ttu-id="aa709-133">[!code-xml[Main](web-api-vsc/sample/TodoApi/TodoApi.csproj?highlight=12)]</span><span class="sxs-lookup"><span data-stu-id="aa709-133">[!code-xml[Main](web-api-vsc/sample/TodoApi/TodoApi.csproj?highlight=12)]</span></span>

<span data-ttu-id="aa709-134">Execute `dotnet restore` para baixar e instalar o provedor EF Core InMemory DB.</span><span class="sxs-lookup"><span data-stu-id="aa709-134">Run `dotnet restore` to download and install the EF Core InMemory DB provider.</span></span> <span data-ttu-id="aa709-135">Execute `dotnet restore` no terminal ou insira `⌘⇧P` (macOS) ou `Ctrl+Shift+P` (Linux) no VS Code e, em seguida, digite **.NET**.</span><span class="sxs-lookup"><span data-stu-id="aa709-135">You can run `dotnet restore` from the terminal or enter `⌘⇧P` (macOS) or `Ctrl+Shift+P` (Linux) in VS Code and then type **.NET**.</span></span> <span data-ttu-id="aa709-136">Selecione **.NET: restaurar os pacotes**.</span><span class="sxs-lookup"><span data-stu-id="aa709-136">Select **.NET: Restore Packages**.</span></span>

## <a name="add-a-model-class"></a><span data-ttu-id="aa709-137">Adicionar uma classe de modelo</span><span class="sxs-lookup"><span data-stu-id="aa709-137">Add a model class</span></span>

<span data-ttu-id="aa709-138">Um modelo é um objeto que representa os dados em seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="aa709-138">A model is an object that represents the data in your application.</span></span> <span data-ttu-id="aa709-139">Nesse caso, o único modelo é um item de tarefas pendentes.</span><span class="sxs-lookup"><span data-stu-id="aa709-139">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="aa709-140">Adicione uma pasta chamada *Models*.</span><span class="sxs-lookup"><span data-stu-id="aa709-140">Add a folder named *Models*.</span></span> <span data-ttu-id="aa709-141">Você pode colocar as classes de modelo em qualquer lugar no projeto, mas a pasta *Models* é usada por convenção.</span><span class="sxs-lookup"><span data-stu-id="aa709-141">You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="aa709-142">Adicione uma classe `TodoItem` com o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="aa709-142">Add a `TodoItem` class with the following code:</span></span>

<span data-ttu-id="aa709-143">[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]</span><span class="sxs-lookup"><span data-stu-id="aa709-143">[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]</span></span>

<span data-ttu-id="aa709-144">O banco de dados gera o `Id` quando um `TodoItem` é criado.</span><span class="sxs-lookup"><span data-stu-id="aa709-144">The database generates the `Id` when a `TodoItem` is created.</span></span>

## <a name="create-the-database-context"></a><span data-ttu-id="aa709-145">Criar o contexto de banco de dados</span><span class="sxs-lookup"><span data-stu-id="aa709-145">Create the database context</span></span>

<span data-ttu-id="aa709-146">O *contexto de banco de dados* é a classe principal que coordena a funcionalidade do Entity Framework para determinado modelo de dados.</span><span class="sxs-lookup"><span data-stu-id="aa709-146">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="aa709-147">Você cria essa classe derivando-a da classe `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="aa709-147">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="aa709-148">Adicione uma classe `TodoContext` à pasta *Models*:</span><span class="sxs-lookup"><span data-stu-id="aa709-148">Add a `TodoContext` class in the *Models* folder:</span></span>

<span data-ttu-id="aa709-149">[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]</span><span class="sxs-lookup"><span data-stu-id="aa709-149">[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]</span></span>

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="aa709-150">Adicionar um controlador</span><span class="sxs-lookup"><span data-stu-id="aa709-150">Add a controller</span></span>

<span data-ttu-id="aa709-151">Na pasta *Controllers*, crie uma classe chamada `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="aa709-151">In the *Controllers* folder, create a class named `TodoController`.</span></span> <span data-ttu-id="aa709-152">Adicione o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="aa709-152">Add the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="aa709-153">Iniciar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="aa709-153">Launch the app</span></span>

<span data-ttu-id="aa709-154">No VS Code, pressione F5 para iniciar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="aa709-154">In VS Code, press F5 to launch the app.</span></span> <span data-ttu-id="aa709-155">Navegue para http://localhost:5000/api/todo (o controlador `Todo` que acabamos de criar).</span><span class="sxs-lookup"><span data-stu-id="aa709-155">Navigate to  http://localhost:5000/api/todo   (The `Todo` controller we just created).</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a><span data-ttu-id="aa709-156">Ajuda do Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="aa709-156">Visual Studio Code help</span></span>

- [<span data-ttu-id="aa709-157">Introdução</span><span class="sxs-lookup"><span data-stu-id="aa709-157">Getting started</span></span>](https://code.visualstudio.com/docs)
- [<span data-ttu-id="aa709-158">Depuração</span><span class="sxs-lookup"><span data-stu-id="aa709-158">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
- [<span data-ttu-id="aa709-159">Terminal integrado</span><span class="sxs-lookup"><span data-stu-id="aa709-159">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [<span data-ttu-id="aa709-160">Atalhos de teclado</span><span class="sxs-lookup"><span data-stu-id="aa709-160">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [<span data-ttu-id="aa709-161">Atalhos de teclado do Mac</span><span class="sxs-lookup"><span data-stu-id="aa709-161">Mac keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [<span data-ttu-id="aa709-162">Atalhos de teclado do Linux</span><span class="sxs-lookup"><span data-stu-id="aa709-162">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [<span data-ttu-id="aa709-163">Atalhos de teclado do Windows</span><span class="sxs-lookup"><span data-stu-id="aa709-163">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

[!INCLUDE[next steps](../includes/webApi/next.md)]


