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
ms.openlocfilehash: abe088f2c9df94135209ce71540e6b345186ee70
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2017
---
# <a name="create-a-web-api-with-aspnet-core-mvc-and-visual-studio-code-on-linux-macos-and-windows"></a><span data-ttu-id="2633f-104">Criar uma API Web com o ASP.NET Core MVC e o Visual Studio Code no Linux, macOS e Windows</span><span class="sxs-lookup"><span data-stu-id="2633f-104">Create a Web API with ASP.NET Core MVC and Visual Studio Code on Linux, macOS, and Windows</span></span>

<span data-ttu-id="2633f-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT) e [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="2633f-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="2633f-106">Neste tutorial, você compilará uma API Web para gerenciar uma lista de itens de “tarefas pendentes”.</span><span class="sxs-lookup"><span data-stu-id="2633f-106">In this tutorial, you’ll build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="2633f-107">Você não compilará uma interface do usuário.</span><span class="sxs-lookup"><span data-stu-id="2633f-107">You won’t build a UI.</span></span>

<span data-ttu-id="2633f-108">Há três versões deste tutorial:</span><span class="sxs-lookup"><span data-stu-id="2633f-108">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="2633f-109">macOS, Linux, Windows: API Web com o Visual Studio Code (Este tutorial)</span><span class="sxs-lookup"><span data-stu-id="2633f-109">macOS, Linux, Windows: Web API with Visual Studio Code (This tutorial)</span></span>
* <span data-ttu-id="2633f-110">macOS: [API Web com o Visual Studio para Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="2633f-110">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="2633f-111">Windows: [API Web com o Visual Studio para Windows](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="2633f-111">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

## <a name="set-up-your-development-environment"></a><span data-ttu-id="2633f-112">Configurar o ambiente de desenvolvimento</span><span class="sxs-lookup"><span data-stu-id="2633f-112">Set up your development environment</span></span>

<span data-ttu-id="2633f-113">Baixe e instale:</span><span class="sxs-lookup"><span data-stu-id="2633f-113">Download and install:</span></span>
- [<span data-ttu-id="2633f-114">.NET Core</span><span class="sxs-lookup"><span data-stu-id="2633f-114">.NET Core</span></span>](https://microsoft.com/net/core)
- [<span data-ttu-id="2633f-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2633f-115">Visual Studio Code</span></span>](https://code.visualstudio.com)
- <span data-ttu-id="2633f-116">[Extensão do C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) do Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2633f-116">Visual Studio Code [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span></span>

## <a name="create-the-project"></a><span data-ttu-id="2633f-117">Criar o projeto</span><span class="sxs-lookup"><span data-stu-id="2633f-117">Create the project</span></span>

<span data-ttu-id="2633f-118">Em um console, execute os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="2633f-118">From a console, run the following commands:</span></span>

```console
mkdir TodoApi
cd TodoApi
dotnet new webapi
```

<span data-ttu-id="2633f-119">Abra a pasta *TodoApi* no Visual Studio Code (VS Code) e selecione o arquivo *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="2633f-119">Open the *TodoApi* folder in Visual Studio Code (VS Code) and select the *Startup.cs* file.</span></span>

- <span data-ttu-id="2633f-120">Selecione **Sim** para a mensagem de **Aviso** “Os ativos necessários para compilar e depurar estão ausentes em 'TodoApi'.</span><span class="sxs-lookup"><span data-stu-id="2633f-120">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'TodoApi'.</span></span> <span data-ttu-id="2633f-121">Deseja adicioná-los?”</span><span class="sxs-lookup"><span data-stu-id="2633f-121">Add them?"</span></span>
- <span data-ttu-id="2633f-122">Selecione **Restaurar** para a mensagem **Informações** “Há dependências não resolvidas”.</span><span class="sxs-lookup"><span data-stu-id="2633f-122">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

![VS Code com o Aviso – Os ativos necessários para compilar e depurar estão ausentes em “TodoApi”.](web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="2633f-126">Pressione **Depurar** (F5) para compilar e executar o programa.</span><span class="sxs-lookup"><span data-stu-id="2633f-126">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="2633f-127">Em um navegador, navegue para http://localhost:5000/api/values.</span><span class="sxs-lookup"><span data-stu-id="2633f-127">In a browser navigate to http://localhost:5000/api/values .</span></span> <span data-ttu-id="2633f-128">O seguinte é exibido:</span><span class="sxs-lookup"><span data-stu-id="2633f-128">The following is displayed:</span></span>

`["value1","value2"]`

<span data-ttu-id="2633f-129">Consulte [Ajuda do Visual Studio Code](#visual-studio-code-help) para obter dicas sobre como usar o VS Code.</span><span class="sxs-lookup"><span data-stu-id="2633f-129">See [Visual Studio Code help](#visual-studio-code-help) for tips on using VS Code.</span></span>

## <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="2633f-130">Adicionar suporte ao Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="2633f-130">Add support for Entity Framework Core</span></span>

<span data-ttu-id="2633f-131">Edite o arquivo *TodoApi.csproj* para instalar o provedor de banco de dados [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/).</span><span class="sxs-lookup"><span data-stu-id="2633f-131">Edit the *TodoApi.csproj* file to install the [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) database provider.</span></span> <span data-ttu-id="2633f-132">Este provedor de banco de dados permite que o Entity Framework Core seja usado com um banco de dados em memória.</span><span class="sxs-lookup"><span data-stu-id="2633f-132">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

<span data-ttu-id="2633f-133">[!code-xml[Main](web-api-vsc/sample/TodoApi/TodoApi.csproj?highlight=12)]</span><span class="sxs-lookup"><span data-stu-id="2633f-133">[!code-xml[Main](web-api-vsc/sample/TodoApi/TodoApi.csproj?highlight=12)]</span></span>

<span data-ttu-id="2633f-134">Execute `dotnet restore` para baixar e instalar o provedor EF Core InMemory DB.</span><span class="sxs-lookup"><span data-stu-id="2633f-134">Run `dotnet restore` to download and install the EF Core InMemory DB provider.</span></span> <span data-ttu-id="2633f-135">Execute `dotnet restore` no terminal ou insira `⌘⇧P` (macOS) ou `Ctrl+Shift+P` (Linux) no VS Code e, em seguida, digite **.NET**.</span><span class="sxs-lookup"><span data-stu-id="2633f-135">You can run `dotnet restore` from the terminal or enter `⌘⇧P` (macOS) or `Ctrl+Shift+P` (Linux) in VS Code and then type **.NET**.</span></span> <span data-ttu-id="2633f-136">Selecione **.NET: restaurar os pacotes**.</span><span class="sxs-lookup"><span data-stu-id="2633f-136">Select **.NET: Restore Packages**.</span></span>

## <a name="add-a-model-class"></a><span data-ttu-id="2633f-137">Adicionar uma classe de modelo</span><span class="sxs-lookup"><span data-stu-id="2633f-137">Add a model class</span></span>

<span data-ttu-id="2633f-138">Um modelo é um objeto que representa os dados em seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="2633f-138">A model is an object that represents the data in your application.</span></span> <span data-ttu-id="2633f-139">Nesse caso, o único modelo é um item de tarefas pendentes.</span><span class="sxs-lookup"><span data-stu-id="2633f-139">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="2633f-140">Adicione uma pasta chamada *Models*.</span><span class="sxs-lookup"><span data-stu-id="2633f-140">Add a folder named *Models*.</span></span> <span data-ttu-id="2633f-141">Você pode colocar as classes de modelo em qualquer lugar no projeto, mas a pasta *Models* é usada por convenção.</span><span class="sxs-lookup"><span data-stu-id="2633f-141">You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="2633f-142">Adicione uma classe `TodoItem` com o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="2633f-142">Add a `TodoItem` class with the following code:</span></span>

<span data-ttu-id="2633f-143">[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]</span><span class="sxs-lookup"><span data-stu-id="2633f-143">[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]</span></span>

<span data-ttu-id="2633f-144">O banco de dados gera o `Id` quando um `TodoItem` é criado.</span><span class="sxs-lookup"><span data-stu-id="2633f-144">The database generates the `Id` when a `TodoItem` is created.</span></span>

## <a name="create-the-database-context"></a><span data-ttu-id="2633f-145">Criar o contexto de banco de dados</span><span class="sxs-lookup"><span data-stu-id="2633f-145">Create the database context</span></span>

<span data-ttu-id="2633f-146">O *contexto de banco de dados* é a classe principal que coordena a funcionalidade do Entity Framework para determinado modelo de dados.</span><span class="sxs-lookup"><span data-stu-id="2633f-146">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="2633f-147">Você cria essa classe derivando-a da classe `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="2633f-147">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="2633f-148">Adicione uma classe `TodoContext` à pasta *Models*:</span><span class="sxs-lookup"><span data-stu-id="2633f-148">Add a `TodoContext` class in the *Models* folder:</span></span>

<span data-ttu-id="2633f-149">[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]</span><span class="sxs-lookup"><span data-stu-id="2633f-149">[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]</span></span>

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="2633f-150">Adicionar um controlador</span><span class="sxs-lookup"><span data-stu-id="2633f-150">Add a controller</span></span>

<span data-ttu-id="2633f-151">Na pasta *Controllers*, crie uma classe chamada `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="2633f-151">In the *Controllers* folder, create a class named `TodoController`.</span></span> <span data-ttu-id="2633f-152">Adicione o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="2633f-152">Add the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="2633f-153">Iniciar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="2633f-153">Launch the app</span></span>

<span data-ttu-id="2633f-154">No VS Code, pressione F5 para iniciar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="2633f-154">In VS Code, press F5 to launch the app.</span></span> <span data-ttu-id="2633f-155">Navegue para http://localhost:5000/api/todo (o controlador `Todo` que acabamos de criar).</span><span class="sxs-lookup"><span data-stu-id="2633f-155">Navigate to  http://localhost:5000/api/todo   (The `Todo` controller we just created).</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a><span data-ttu-id="2633f-156">Ajuda do Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2633f-156">Visual Studio Code help</span></span>

- [<span data-ttu-id="2633f-157">Introdução</span><span class="sxs-lookup"><span data-stu-id="2633f-157">Getting started</span></span>](https://code.visualstudio.com/docs)
- [<span data-ttu-id="2633f-158">Depuração</span><span class="sxs-lookup"><span data-stu-id="2633f-158">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
- [<span data-ttu-id="2633f-159">Terminal integrado</span><span class="sxs-lookup"><span data-stu-id="2633f-159">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [<span data-ttu-id="2633f-160">Atalhos de teclado</span><span class="sxs-lookup"><span data-stu-id="2633f-160">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [<span data-ttu-id="2633f-161">Atalhos de teclado do Mac</span><span class="sxs-lookup"><span data-stu-id="2633f-161">Mac keyboard shortcuts</span></span>](https://go.microsoft.com/fwlink/?linkid=832143)
  - [<span data-ttu-id="2633f-162">Atalhos de teclado do Linux</span><span class="sxs-lookup"><span data-stu-id="2633f-162">Linux keyboard shortcuts</span></span>](https://go.microsoft.com/fwlink/?linkid=832144)
  - [<span data-ttu-id="2633f-163">Atalhos de teclado do Windows</span><span class="sxs-lookup"><span data-stu-id="2633f-163">Windows keyboard shortcuts</span></span>](https://go.microsoft.com/fwlink/?linkid=832145)

[!INCLUDE[next steps](../includes/webApi/next.md)]


