---
title: Criar uma API Web com o ASP.NET Core e o VS Code
author: rick-anderson
description: Compilar uma API Web no macOS, Linux ou Windows com o ASP.NET Core MVC e o Visual Studio Code
manager: wpickett
ms.author: riande
ms.date: 09/22/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/web-api-vsc
ms.openlocfilehash: 44566c4014400aa2ca3d512eeaa226637b5f0b97
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2018
---
# <a name="create-a-web-api-with-aspnet-core-mvc-and-visual-studio-code-on-linux-macos-and-windows"></a><span data-ttu-id="6be70-103">Criar uma API Web com o ASP.NET Core MVC e o Visual Studio Code no Linux, macOS e Windows</span><span class="sxs-lookup"><span data-stu-id="6be70-103">Create a Web API with ASP.NET Core MVC and Visual Studio Code on Linux, macOS, and Windows</span></span>

<span data-ttu-id="6be70-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT) e [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="6be70-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="6be70-105">Neste tutorial, compile uma API Web para gerenciar uma lista de itens de "tarefas pendentes".</span><span class="sxs-lookup"><span data-stu-id="6be70-105">In this tutorial, build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="6be70-106">Uma interface do usuário não é construída.</span><span class="sxs-lookup"><span data-stu-id="6be70-106">A UI isn't constructed.</span></span>

<span data-ttu-id="6be70-107">Há três versões deste tutorial:</span><span class="sxs-lookup"><span data-stu-id="6be70-107">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="6be70-108">macOS, Linux, Windows: API Web com o Visual Studio Code (Este tutorial)</span><span class="sxs-lookup"><span data-stu-id="6be70-108">macOS, Linux, Windows: Web API with Visual Studio Code (This tutorial)</span></span>
* <span data-ttu-id="6be70-109">macOS: [API Web com o Visual Studio para Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="6be70-109">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="6be70-110">Windows: [API Web com o Visual Studio para Windows](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="6be70-110">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

## <a name="set-up-your-development-environment"></a><span data-ttu-id="6be70-111">Configurar o ambiente de desenvolvimento</span><span class="sxs-lookup"><span data-stu-id="6be70-111">Set up your development environment</span></span>

<span data-ttu-id="6be70-112">Baixe e instale:</span><span class="sxs-lookup"><span data-stu-id="6be70-112">Download and install:</span></span>
- <span data-ttu-id="6be70-113">[SDK do .NET Core 2.0.0](https://www.microsoft.com/net/core) ou posterior.</span><span class="sxs-lookup"><span data-stu-id="6be70-113">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span>
- [<span data-ttu-id="6be70-114">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6be70-114">Visual Studio Code</span></span>](https://code.visualstudio.com)
- <span data-ttu-id="6be70-115">[Extensão do C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) do Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6be70-115">Visual Studio Code [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span></span>

## <a name="create-the-project"></a><span data-ttu-id="6be70-116">Criar o projeto</span><span class="sxs-lookup"><span data-stu-id="6be70-116">Create the project</span></span>

<span data-ttu-id="6be70-117">Em um console, execute os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="6be70-117">From a console, run the following commands:</span></span>

```console
mkdir TodoApi
cd TodoApi
dotnet new webapi
```

<span data-ttu-id="6be70-118">Abra a pasta *TodoApi* no Visual Studio Code (VS Code) e selecione o arquivo *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="6be70-118">Open the *TodoApi* folder in Visual Studio Code (VS Code) and select the *Startup.cs* file.</span></span>

- <span data-ttu-id="6be70-119">Selecione **Sim** para a mensagem de **Aviso** “Os ativos necessários para compilar e depurar estão ausentes em 'TodoApi'.</span><span class="sxs-lookup"><span data-stu-id="6be70-119">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'TodoApi'.</span></span> <span data-ttu-id="6be70-120">Deseja adicioná-los?”</span><span class="sxs-lookup"><span data-stu-id="6be70-120">Add them?"</span></span>
- <span data-ttu-id="6be70-121">Selecione **Restaurar** para a mensagem **Informações** “Há dependências não resolvidas”.</span><span class="sxs-lookup"><span data-stu-id="6be70-121">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

![VS Code com o Aviso – Os ativos necessários para compilar e depurar estão ausentes em “TodoApi”.](web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="6be70-125">Pressione **Depurar** (F5) para compilar e executar o programa.</span><span class="sxs-lookup"><span data-stu-id="6be70-125">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="6be70-126">Em um navegador, navegue para http://localhost:5000/api/values.</span><span class="sxs-lookup"><span data-stu-id="6be70-126">In a browser navigate to http://localhost:5000/api/values .</span></span> <span data-ttu-id="6be70-127">O seguinte é exibido:</span><span class="sxs-lookup"><span data-stu-id="6be70-127">The following is displayed:</span></span>

`["value1","value2"]`

<span data-ttu-id="6be70-128">Consulte [Ajuda do Visual Studio Code](#visual-studio-code-help) para obter dicas sobre como usar o VS Code.</span><span class="sxs-lookup"><span data-stu-id="6be70-128">See [Visual Studio Code help](#visual-studio-code-help) for tips on using VS Code.</span></span>

## <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="6be70-129">Adicionar suporte ao Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="6be70-129">Add support for Entity Framework Core</span></span>

<span data-ttu-id="6be70-130">A criação de um novo projeto no .NET Core 2.0 adiciona o provedor “Microsoft.AspNetCore.All” ao arquivo *TodoApi.csproj*.</span><span class="sxs-lookup"><span data-stu-id="6be70-130">Creating a new project in .NET Core 2.0 adds the 'Microsoft.AspNetCore.All' provider in the *TodoApi.csproj* file.</span></span> <span data-ttu-id="6be70-131">Não é necessário instalar o provedor de banco de dados [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) separadamente.</span><span class="sxs-lookup"><span data-stu-id="6be70-131">There's no need to install the [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) database provider separately.</span></span> <span data-ttu-id="6be70-132">Este provedor de banco de dados permite que o Entity Framework Core seja usado com um banco de dados em memória.</span><span class="sxs-lookup"><span data-stu-id="6be70-132">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

[!code-xml[Main](web-api-vsc/sample/TodoApi/TodoApi.csproj?highlight=12)]

## <a name="add-a-model-class"></a><span data-ttu-id="6be70-133">Adicionar uma classe de modelo</span><span class="sxs-lookup"><span data-stu-id="6be70-133">Add a model class</span></span>

<span data-ttu-id="6be70-134">Um modelo é um objeto que representa os dados em seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="6be70-134">A model is an object that represents the data in your application.</span></span> <span data-ttu-id="6be70-135">Nesse caso, o único modelo é um item de tarefas pendentes.</span><span class="sxs-lookup"><span data-stu-id="6be70-135">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="6be70-136">Adicione uma pasta chamada *Models*.</span><span class="sxs-lookup"><span data-stu-id="6be70-136">Add a folder named *Models*.</span></span> <span data-ttu-id="6be70-137">Você pode colocar as classes de modelo em qualquer lugar no projeto, mas a pasta *Models* é usada por convenção.</span><span class="sxs-lookup"><span data-stu-id="6be70-137">You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="6be70-138">Adicione uma classe `TodoItem` com o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="6be70-138">Add a `TodoItem` class with the following code:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="6be70-139">O banco de dados gera o `Id` quando um `TodoItem` é criado.</span><span class="sxs-lookup"><span data-stu-id="6be70-139">The database generates the `Id` when a `TodoItem` is created.</span></span>

## <a name="create-the-database-context"></a><span data-ttu-id="6be70-140">Criar o contexto de banco de dados</span><span class="sxs-lookup"><span data-stu-id="6be70-140">Create the database context</span></span>

<span data-ttu-id="6be70-141">O *contexto de banco de dados* é a classe principal que coordena a funcionalidade do Entity Framework para determinado modelo de dados.</span><span class="sxs-lookup"><span data-stu-id="6be70-141">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="6be70-142">Você cria essa classe derivando-a da classe `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="6be70-142">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="6be70-143">Adicione uma classe `TodoContext` à pasta *Models*:</span><span class="sxs-lookup"><span data-stu-id="6be70-143">Add a `TodoContext` class in the *Models* folder:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="6be70-144">Adicionar um controlador</span><span class="sxs-lookup"><span data-stu-id="6be70-144">Add a controller</span></span>

<span data-ttu-id="6be70-145">Na pasta *Controllers*, crie uma classe chamada `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="6be70-145">In the *Controllers* folder, create a class named `TodoController`.</span></span> <span data-ttu-id="6be70-146">Adicione o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="6be70-146">Add the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="6be70-147">Iniciar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="6be70-147">Launch the app</span></span>

<span data-ttu-id="6be70-148">No VS Code, pressione F5 para iniciar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="6be70-148">In VS Code, press F5 to launch the app.</span></span> <span data-ttu-id="6be70-149">Navegue para http://localhost:5000/api/todo (o controlador `Todo` que acabamos de criar).</span><span class="sxs-lookup"><span data-stu-id="6be70-149">Navigate to  http://localhost:5000/api/todo   (The `Todo` controller we just created).</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a><span data-ttu-id="6be70-150">Ajuda do Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6be70-150">Visual Studio Code help</span></span>

- [<span data-ttu-id="6be70-151">Introdução</span><span class="sxs-lookup"><span data-stu-id="6be70-151">Getting started</span></span>](https://code.visualstudio.com/docs)
- [<span data-ttu-id="6be70-152">Depuração</span><span class="sxs-lookup"><span data-stu-id="6be70-152">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
- [<span data-ttu-id="6be70-153">Terminal integrado</span><span class="sxs-lookup"><span data-stu-id="6be70-153">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [<span data-ttu-id="6be70-154">Atalhos de teclado</span><span class="sxs-lookup"><span data-stu-id="6be70-154">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [<span data-ttu-id="6be70-155">Atalhos de teclado do Mac</span><span class="sxs-lookup"><span data-stu-id="6be70-155">Mac keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [<span data-ttu-id="6be70-156">Atalhos de teclado do Linux</span><span class="sxs-lookup"><span data-stu-id="6be70-156">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [<span data-ttu-id="6be70-157">Atalhos de teclado do Windows</span><span class="sxs-lookup"><span data-stu-id="6be70-157">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

[!INCLUDE[next steps](../includes/webApi/next.md)]


