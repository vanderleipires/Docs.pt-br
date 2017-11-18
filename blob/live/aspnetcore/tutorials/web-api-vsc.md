---
title: Criar uma API Web com o ASP.NET Core e o VS Code
description: Compilar uma API Web no macOS, Linux ou Windows com o ASP.NET Core MVC e o Visual Studio Code
author: rick-anderson
ms.author: riande
ms.date: 09/22/2017
ms.topic: get-started-article
ms.prod: asp.net-core
ms.technology: aspnet
keywords: "ASP.NET Core, WebAPI, API Web, REST, Mac, Linux, HTTP, Serviço, Serviço HTTP, VS Code"
manager: wpickett
ms.assetid: 830b4bf5-dd14-423e-9f59-764a6f13a8f6
uid: tutorials/web-api-vsc
ms.openlocfilehash: caf40ee1c2d45d2fbf33b07d707fa4f1be98d31c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
# <a name="create-a-web-api-with-aspnet-core-mvc-and-visual-studio-code-on-linux-macos-and-windows"></a><span data-ttu-id="ff03d-104">Criar uma API Web com o ASP.NET Core MVC e o Visual Studio Code no Linux, macOS e Windows</span><span class="sxs-lookup"><span data-stu-id="ff03d-104">Create a Web API with ASP.NET Core MVC and Visual Studio Code on Linux, macOS, and Windows</span></span>

<span data-ttu-id="ff03d-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT) e [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="ff03d-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="ff03d-106">Neste tutorial, você compilará uma API Web para gerenciar uma lista de itens de “tarefas pendentes”.</span><span class="sxs-lookup"><span data-stu-id="ff03d-106">In this tutorial, you’ll build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="ff03d-107">Você não compilará uma interface do usuário.</span><span class="sxs-lookup"><span data-stu-id="ff03d-107">You won’t build a UI.</span></span>

<span data-ttu-id="ff03d-108">Há três versões deste tutorial:</span><span class="sxs-lookup"><span data-stu-id="ff03d-108">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="ff03d-109">macOS, Linux, Windows: API Web com o Visual Studio Code (Este tutorial)</span><span class="sxs-lookup"><span data-stu-id="ff03d-109">macOS, Linux, Windows: Web API with Visual Studio Code (This tutorial)</span></span>
* <span data-ttu-id="ff03d-110">macOS: [API Web com o Visual Studio para Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="ff03d-110">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="ff03d-111">Windows: [API Web com o Visual Studio para Windows](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="ff03d-111">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

## <a name="set-up-your-development-environment"></a><span data-ttu-id="ff03d-112">Configurar o ambiente de desenvolvimento</span><span class="sxs-lookup"><span data-stu-id="ff03d-112">Set up your development environment</span></span>

<span data-ttu-id="ff03d-113">Baixe e instale:</span><span class="sxs-lookup"><span data-stu-id="ff03d-113">Download and install:</span></span>
- <span data-ttu-id="ff03d-114">[SDK do .NET Core 2.0.0](https://www.microsoft.com/net/core) ou posterior.</span><span class="sxs-lookup"><span data-stu-id="ff03d-114">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span>
- [<span data-ttu-id="ff03d-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ff03d-115">Visual Studio Code</span></span>](https://code.visualstudio.com)
- <span data-ttu-id="ff03d-116">[Extensão do C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) do Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ff03d-116">Visual Studio Code [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span></span>

## <a name="create-the-project"></a><span data-ttu-id="ff03d-117">Criar o projeto</span><span class="sxs-lookup"><span data-stu-id="ff03d-117">Create the project</span></span>

<span data-ttu-id="ff03d-118">Em um console, execute os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="ff03d-118">From a console, run the following commands:</span></span>

```console
mkdir TodoApi
cd TodoApi
dotnet new webapi
```

<span data-ttu-id="ff03d-119">Abra a pasta *TodoApi* no Visual Studio Code (VS Code) e selecione o arquivo *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="ff03d-119">Open the *TodoApi* folder in Visual Studio Code (VS Code) and select the *Startup.cs* file.</span></span>

- <span data-ttu-id="ff03d-120">Selecione **Sim** para a mensagem de **Aviso** “Os ativos necessários para compilar e depurar estão ausentes em 'TodoApi'.</span><span class="sxs-lookup"><span data-stu-id="ff03d-120">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'TodoApi'.</span></span> <span data-ttu-id="ff03d-121">Deseja adicioná-los?”</span><span class="sxs-lookup"><span data-stu-id="ff03d-121">Add them?"</span></span>
- <span data-ttu-id="ff03d-122">Selecione **Restaurar** para a mensagem **Informações** “Há dependências não resolvidas”.</span><span class="sxs-lookup"><span data-stu-id="ff03d-122">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

![VS Code com o Aviso – Os ativos necessários para compilar e depurar estão ausentes em “TodoApi”.](web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="ff03d-126">Pressione **Depurar** (F5) para compilar e executar o programa.</span><span class="sxs-lookup"><span data-stu-id="ff03d-126">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="ff03d-127">Em um navegador, navegue para http://localhost:5000/api/values.</span><span class="sxs-lookup"><span data-stu-id="ff03d-127">In a browser navigate to http://localhost:5000/api/values .</span></span> <span data-ttu-id="ff03d-128">O seguinte é exibido:</span><span class="sxs-lookup"><span data-stu-id="ff03d-128">The following is displayed:</span></span>

`["value1","value2"]`

<span data-ttu-id="ff03d-129">Consulte [Ajuda do Visual Studio Code](#visual-studio-code-help) para obter dicas sobre como usar o VS Code.</span><span class="sxs-lookup"><span data-stu-id="ff03d-129">See [Visual Studio Code help](#visual-studio-code-help) for tips on using VS Code.</span></span>

## <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="ff03d-130">Adicionar suporte ao Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="ff03d-130">Add support for Entity Framework Core</span></span>

<span data-ttu-id="ff03d-131">Edite o arquivo *TodoApi.csproj* para instalar o provedor de banco de dados [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/).</span><span class="sxs-lookup"><span data-stu-id="ff03d-131">Edit the *TodoApi.csproj* file to install the [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) database provider.</span></span> <span data-ttu-id="ff03d-132">Este provedor de banco de dados permite que o Entity Framework Core seja usado com um banco de dados em memória.</span><span class="sxs-lookup"><span data-stu-id="ff03d-132">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

[!code-xml[Main](web-api-vsc/sample/TodoApi/TodoApi.csproj?highlight=12)]

<span data-ttu-id="ff03d-133">Execute `dotnet restore` para baixar e instalar o provedor EF Core InMemory DB.</span><span class="sxs-lookup"><span data-stu-id="ff03d-133">Run `dotnet restore` to download and install the EF Core InMemory DB provider.</span></span> <span data-ttu-id="ff03d-134">Execute `dotnet restore` no terminal ou insira `⌘⇧P` (macOS) ou `Ctrl+Shift+P` (Linux) no VS Code e, em seguida, digite **.NET**.</span><span class="sxs-lookup"><span data-stu-id="ff03d-134">You can run `dotnet restore` from the terminal or enter `⌘⇧P` (macOS) or `Ctrl+Shift+P` (Linux) in VS Code and then type **.NET**.</span></span> <span data-ttu-id="ff03d-135">Selecione **.NET: restaurar os pacotes**.</span><span class="sxs-lookup"><span data-stu-id="ff03d-135">Select **.NET: Restore Packages**.</span></span>

## <a name="add-a-model-class"></a><span data-ttu-id="ff03d-136">Adicionar uma classe de modelo</span><span class="sxs-lookup"><span data-stu-id="ff03d-136">Add a model class</span></span>

<span data-ttu-id="ff03d-137">Um modelo é um objeto que representa os dados em seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="ff03d-137">A model is an object that represents the data in your application.</span></span> <span data-ttu-id="ff03d-138">Nesse caso, o único modelo é um item de tarefas pendentes.</span><span class="sxs-lookup"><span data-stu-id="ff03d-138">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="ff03d-139">Adicione uma pasta chamada *Models*.</span><span class="sxs-lookup"><span data-stu-id="ff03d-139">Add a folder named *Models*.</span></span> <span data-ttu-id="ff03d-140">Você pode colocar as classes de modelo em qualquer lugar no projeto, mas a pasta *Models* é usada por convenção.</span><span class="sxs-lookup"><span data-stu-id="ff03d-140">You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="ff03d-141">Adicione uma classe `TodoItem` com o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="ff03d-141">Add a `TodoItem` class with the following code:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="ff03d-142">O banco de dados gera o `Id` quando um `TodoItem` é criado.</span><span class="sxs-lookup"><span data-stu-id="ff03d-142">The database generates the `Id` when a `TodoItem` is created.</span></span>

## <a name="create-the-database-context"></a><span data-ttu-id="ff03d-143">Criar o contexto de banco de dados</span><span class="sxs-lookup"><span data-stu-id="ff03d-143">Create the database context</span></span>

<span data-ttu-id="ff03d-144">O *contexto de banco de dados* é a classe principal que coordena a funcionalidade do Entity Framework para determinado modelo de dados.</span><span class="sxs-lookup"><span data-stu-id="ff03d-144">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="ff03d-145">Você cria essa classe derivando-a da classe `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="ff03d-145">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="ff03d-146">Adicione uma classe `TodoContext` à pasta *Models*:</span><span class="sxs-lookup"><span data-stu-id="ff03d-146">Add a `TodoContext` class in the *Models* folder:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="ff03d-147">Adicionar um controlador</span><span class="sxs-lookup"><span data-stu-id="ff03d-147">Add a controller</span></span>

<span data-ttu-id="ff03d-148">Na pasta *Controllers*, crie uma classe chamada `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="ff03d-148">In the *Controllers* folder, create a class named `TodoController`.</span></span> <span data-ttu-id="ff03d-149">Adicione o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="ff03d-149">Add the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="ff03d-150">Iniciar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="ff03d-150">Launch the app</span></span>

<span data-ttu-id="ff03d-151">No VS Code, pressione F5 para iniciar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="ff03d-151">In VS Code, press F5 to launch the app.</span></span> <span data-ttu-id="ff03d-152">Navegue para http://localhost:5000/api/todo (o controlador `Todo` que acabamos de criar).</span><span class="sxs-lookup"><span data-stu-id="ff03d-152">Navigate to  http://localhost:5000/api/todo   (The `Todo` controller we just created).</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a><span data-ttu-id="ff03d-153">Ajuda do Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ff03d-153">Visual Studio Code help</span></span>

- [<span data-ttu-id="ff03d-154">Introdução</span><span class="sxs-lookup"><span data-stu-id="ff03d-154">Getting started</span></span>](https://code.visualstudio.com/docs)
- [<span data-ttu-id="ff03d-155">Depuração</span><span class="sxs-lookup"><span data-stu-id="ff03d-155">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
- [<span data-ttu-id="ff03d-156">Terminal integrado</span><span class="sxs-lookup"><span data-stu-id="ff03d-156">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [<span data-ttu-id="ff03d-157">Atalhos de teclado</span><span class="sxs-lookup"><span data-stu-id="ff03d-157">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [<span data-ttu-id="ff03d-158">Atalhos de teclado do Mac</span><span class="sxs-lookup"><span data-stu-id="ff03d-158">Mac keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [<span data-ttu-id="ff03d-159">Atalhos de teclado do Linux</span><span class="sxs-lookup"><span data-stu-id="ff03d-159">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [<span data-ttu-id="ff03d-160">Atalhos de teclado do Windows</span><span class="sxs-lookup"><span data-stu-id="ff03d-160">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

[!INCLUDE[next steps](../includes/webApi/next.md)]


